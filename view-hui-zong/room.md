# Room

* 实体类 \`@Entity\`

```java
@Entity
public class User {

    @PrimaryKey(autoGenerate = true)
    public int id;

    public String firstName;
    public String lastName;
    public String defa;


    @Ignore
    public String other;

    @Ignore
    public String other2;

    public User() {
    }

    public User(String firstName, String lastName, String other) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.other = other;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", firstName='" + firstName + '\'' +
                ", lastName='" + lastName + '\'' +
                ", defa='" + defa + '\'' +
                ", other='" + other + '\'' +
                '}';
    }
}
```

> 必须有无参构造 如果需要制定某个字段不作为表中的一列需要添加@Ignore注解@PrimaryKey\(autoGenerate = true\)  主键，自增长

* Dao层

```java
@Dao
public interface UserDao {

    /**
     * 插入User，返回插入的rowId
     * @param users
     * @return 插入的rowId
     */
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    List<Long> insertUser(User ... users);

    /**
     * 更新User，返回更新了多少行
     * @param users
     * @return 更新的行数
     */
    @Update(onConflict = OnConflictStrategy.REPLACE)
    int updateUser(User ... users);

    /**
     * 删除User
     * @param users
     * @return 删除的行数
     */
    @Delete
    int deleteUser(User ... users);

    /**
     * 查询所有User
     * @return
     */
    @Query("SELECT * FROM user")
    List<User> loadAllUser();
}
```

> 冲突时
>
> ```java
> (onConflict = OnConflictStrategy.REPLACE)
> ```

* dataBase

```java
@Database(entities = {User.class}, version = 2, exportSchema = false)
public abstract class AppDataBase extends RoomDatabase {

    public abstract UserDao userDao();
}
```

> 必须继承 RoomDatabase
>
> 指明entities，版本号

* Application中创建全局对象

```java
        // 数据库升级1 -> 2 新增了age列
        Migration MIGRATION_1_2 = new Migration(1, 2) {
            @Override
            public void migrate(@NonNull SupportSQLiteDatabase database) {
                database.execSQL("ALTER TABLE User ADD COLUMN defa varchar");
            }
        };
        mBuild = Room.databaseBuilder(getApplicationContext(),
                AppDataBase.class,
                "user_wlhy.db")
                .allowMainThreadQueries()
                .addMigrations(MIGRATION_1_2)
                .build();
```

