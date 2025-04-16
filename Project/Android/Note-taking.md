# Android 数据库访问设计：

将数据库操作封装在 Kotlin `object` 单例中适合小项目，但在复杂项目中推荐使用 Repository 模式 + ViewModel + 依赖注入 的架构方式以提升可维护性和测试性。

## 单例封装设计（使用 Kotlin `object`）

### 优点
- 简洁易用，**全局唯一**；
- 无需手动创建实例；
- 天然**线程安全**；
- 初学者易于上手。

### 缺点
- 不利于**单元测试**（无法 mock）；
- **UI层**可能直接依赖**数据层**，**耦合性**高 High coupling；
- 不支持**扩展多数据源**；
- 生命周期不可控，可能引发内存泄漏（若持有 Activity Context）；

### 示例代码
```kotlin
object NoteRepository {
    private val db = NoteDatabase.getInstance()
    
    fun insertNote(note: Note) { ... }
    fun getAllNotes(): List<Note> { ... }
}
```

## Repository 架构模式

<img width="179" alt="Screenshot 2025-04-16 at 1 07 32 AM" src="https://github.com/user-attachments/assets/747f1eb1-39e7-4b5a-8d8e-e2fcf5b9b63d" />

- 使用`Repository`模式统一管理**数据访问逻辑**，通过**依赖注入**将其传入`ViewModel`；
- 隐藏底层实现（数据库 / 网络），为上层提供统一的**数据接口**。如果未来要切换数据源，比如从 Room 切到网络，只需要修改 Repository 内部实现即可，上层完全不受影响。

### 优点
- 解耦**视图层**与**数据层**；
- 支持**单元测试**（可 **mock** Repository）；
- 易于**维护**和**扩展多个数据源**（如本地缓存 + 网络）；
- 与`MVVM`架构天然兼容；
- 配合**依赖注入**（如 `Hilt`）使用效果更佳。

### 缺点
- 初期搭建结构较**复杂**；
- 对于小型项目可能显得冗余；
- 如果设计不清晰可能造成**职责混乱**（Repository 逻辑膨胀）。

### 示例结构（基于 Room + ViewModel）

```kotlin
// DAO（数据库接口）
@Dao
interface NoteDao {
    @Insert suspend fun insert(note: Note)
    @Query("SELECT * FROM notes") suspend fun getAll(): List<Note>
}

// Repository
class NoteRepository(private val dao: NoteDao) {
    suspend fun insertNote(note: Note) = dao.insert(note)
    suspend fun fetchNotes(): List<Note> = dao.getAll()
}

// ViewModel
class NoteViewModel(private val repository: NoteRepository) : ViewModel() {
    private val _notes = MutableLiveData<List<Note>>()
    val notes: LiveData<List<Note>> = _notes

    fun loadNotes() {
        viewModelScope.launch {
            _notes.value = repository.fetchNotes()
        }
    }
}
```
