
## Migrating to 4.1.0
This version introduces some improvements and additions to chat elements structure. 
Most significant is the addition of explicit string identification property.
Up to this version elements identification was done by their `timestamp`.

### Steps to successful migration:

1. make sure to change imports of ChatElement and StorableChatelement packages to `com.nanorep.convesationui.structure.element`
```kotlin
import com.nanorep.convesationui.structure.elements.ChatElement
import com.nanorep.convesationui.structure.elements.StorableChatElement
```

2. make sure to update StorableChatElement implementing classes, with the addition of `getId` method. 
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

    private var timestamp: Long = 0

    // ... define additional properties

    constructor(groupId: String, storable: StorableChatElement) : this() {
        id = storable.getId()
        timestamp = storable.getTimestamp()
        // ... additional initializations
    }

    override fun getId(): String {
        return id
    }

    fun setId(id: String){
        this.id = id
    }

    // ... additional setters/getters
}

```

3. Updating your implementation of the `ChatElementListener` by adding the new methods, `onRemove(id: String)` and `onUpdate(id: String, item: StorableChatElement)

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
