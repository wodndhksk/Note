```

package com.megait.soir.repository;

import com.megait.soir.domain.Item;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;

import java.util.List;

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

```java
