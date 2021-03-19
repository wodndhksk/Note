```java
@Repository
public interface ItemRepository extends JpaRepository<Item, Long> {

        //상품이름, 브랜드 이름으로 전체 검색
        @Query(value = "select * from  Item e where e.Name like %:keyword% or e.Brand like %:keyword%", nativeQuery = true)
        List<Item> findByAllKeyword(@Param("keyword") String keyword);

        @Query(value = "select * from  Item e where e.Name like %:keyword%", nativeQuery = true)
        List<Item> findByNameKeyword(@Param("keyword") String keyword);

        @Query(value = "select * from  Item e where e.Brand like %:keyword%", nativeQuery = true)
        List<Item> findByBrandKeyword(@Param("keyword") String keyword);
}

```
