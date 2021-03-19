```
           <!-- Search bar -->
            <form class="form-inline my-2 my-lg-0" th:action="@{/}" th:method="get">
                <select id="searchType" name="searchType">
                    <option th:value="item_all" >전체</option>
                    <option th:value="item_name">상품명</option>
                    <option th:value="brand_name" >브랜드명</option>

                </select>
                <input class="form-control mr-sm-2" name="keyword" id="keyword" type="search" placeholder="Search" aria-label="Search">
                <button class="btn btn-outline-light my-2 my-sm-0" type="submit">search</button>
            </form>


```html
