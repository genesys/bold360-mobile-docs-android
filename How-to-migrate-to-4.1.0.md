
## Migrating to 4.1.0
This version introduces some improvements and additions to chat elements structure. 
Most significant is the addition of explicit string identification property, the `id`.   
Up to this version elements identification was done by their `timestamp`.

### Steps to successful migration:

1. **Change imports** of ChatElement and StorableChatelement packages as follows:
```kotlin
import com.nanorep.convesationui.structure.elements.ChatElement
import com.nanorep.convesationui.structure.elements.StorableChatElement
```

2. **Update StorableChatElement implementations** with the addition of `getId` method. 
```kotlin
/*
 code snippet of HistoryElement which we use as chat element storage 
 record presentation using `Room Persistence Library`. 
*/
@Entity
class HistoryElement() : StorableChatElement {

    @PrimaryKey
    @NonNull
    private var id: String = ""

    private var groupId:String = ""

    private var timestamp: Long = 0

    // ... define additional properties

    constructor(groupId: String, storable: StorableChatElement) : this() {
        this.groupId = groupId

        id = storable.getId()
        
        timestamp = storable.getTimestamp()
        
        // ... additional initializations
    }

---------------------------------------
    // implementing the added method:
    override fun getId(): String {
        return id
    }
---------------------------------------
    fun setId(id: String){
        this.id = id
    }

    // ... additional setters/getters
}
```

3. **Update `ChatElementListener` implementation**, by adding the new methods:   
`onRemove(id: String)`   
`onUpdate(id: String, item: StorableChatElement)`

```kotlin
class RoomHistoryProvider : ChatElementListener {

    override fun onUpdate(id: String, item: StorableChatElement) {
        // ... surround with async activation  
        historyDao.update(groupId, id, item.getTimestamp(), item.getStorageKey(), item.getStatus())
    }

    override fun onRemove(id: String) {
        // ... surround with async activation 
        historyDao.delete(groupId, id)
    }
}

// correlating sqlite methods defined as follow:
@Dao
interface HistoryDao {
    // ....

    @Query("UPDATE historyElement SET `key`=:key, `status`=:status, `timestamp`=:timestamp WHERE groupId=:groupId and id=:id")
    suspend fun update(groupId: String, id: String, timestamp: Long, key: ByteArray, status: Int)

    @Query("DELETE FROM historyElement WHERE groupId=:groupId and id=:id")
    suspend fun delete(groupId: String, id: String)
}

// groupId - identifies the chat in which the chat element was created. 
// id - identifies uniquely the chat element. 
```  

4. **Update** as/if needed your **current history support storage.**   
It can be by adding an Id column to a table, or what ever is needed, according to your implementation. After this update, your history implementation should be able to update and remove elements according to their id.

    ```kotlin
    // Using Room migration capabilities:

    // We create a new table with a new Id column and copy to it the previous saved history:

    val migration = object : Migration(oldVer, newVer) {
        override fun migrate(database: SupportSQLiteDatabase) {
            database.execSQL("""Create TABLE new_HistoryElement (
                eId TEXT PRIMARY KEY NOT NULL DEFAULT '',
                groupId TEXT NOT NULL,
                scope INTEGER NOT NULL,
                inDate INTEGER NOT NULL,
                textContent TEXT NOT NULL,
                type INTEGER NOT NULL,
                isStorageReady INTEGER NOT NULL,
                key BLOB NOT NULL,
                timestamp INTEGER NOT NULL,
                status INTEGER NOT NULL DEFAULT -1);""")
            database.execSQL("""INSERT INTO new_HistoryElement (eId, groupId,
                scope,inDate,textContent,type,isStorageReady,key,timestamp, status) 
                SELECT timestamp, groupId,
                scope,inDate,textContent,type,isStorageReady, key,timestamp,status FROM HistoryElement;""")
            database.execSQL("DROP TABLE HistoryElement;")
            database.execSQL("ALTER TABLE new_HistoryElement RENAME TO HistoryElement;")
        }
    }

    return Room.databaseBuilder(context, DB_CLASS_TYPE, DB_NAME)
                .addMigrations(migration)
                .build()
    ```

5. **Convert (migrate) current stored elements to the new StorableChatElement scheme** - _optional_   
<sup>**[see Migration tool](#Migration-tool)**</sup>
---

### Migration tool
We've provided a tool with this SDK to help you convert your current old chat elements to the new scheme. Migration operation should be done once.   

#### How to start the migration
Pass implementation of `ElementMigration` to start the migration operation, as follows:
```kotlin
HistoryMigration.Companion.start(new HistoryMigrationImpl(context));
```
Once started, `onFetch` will be called on your `ElementMigration` implementation, to provide a list of elements that should be converted. `onFetch` will be called again and again, until an empty list was fetched, indicating that there no more elements to convert.

Each fetched block of elements will be converted and passed back on `onReplace(from: Int, migration: Map<String, StorableChatElement>?)` method.   
`migration` maps old elements ids to the new converted `StorablChatElement`, (which should replace the old one on your history support storage).   

If no old elements were found on fetched list, onReplace will be called with empty migration map.

```kotlin
// Our sample implementation:

class HistoryMigrationProvider : ElementMigration {

    // ...

    // fetch block of elements from the elements table 
    // (no matter what the groupId is)
    override fun getCount(toIndex: Int, fromIdx: Int): MutableList<HistoryElement> {
        fetchedChunk = historyDao.getCount(toIndex, fromIdx - toIndex)
        return fetchedChunk
    }
    // Migrated elements are inserted into the table instead the old ones.
    override fun onReplace(from: Int, migration: Map<String, StorableChatElement>?) {
        migration?.forEach {(prevId, storable) ->
            val (groupId, inDate) = fetchedChunk.find { it.getId() == prevId }?.let{ it.groupId to it.inDate}?: null to null
            groupId?.run {
                // delete old element:
                historyDao.delete(this, prevId) 
                // inset new converted element:
                historyDao.insert(HistoryElement(this, storable)...) 
            }
        }
    }
}
```
[see full sample](https://github.com/bold360ai/bold360-mobile-samples-android/blob/master/SDKSamples/app/src/main/java/com/sdk/samples/topics/BotChatHistory.kt)