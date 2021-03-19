```java

@Service
@Transactional // 객체가 사용하는 모든 method에 transaction이 적용된다.
@RequiredArgsConstructor
public class ItemService {

    private final ItemRepository itemRepository;
 
    @PostConstruct
    public void initAlbumItems() throws IOException, ParseException {


    //Get Item by All keyword (name, brand 전체검색)
    public List<Item> findByAllKeyword(String keyword){
        return itemRepository.findByAllKeyword(keyword);
    }

    //Get Item by Name keyword (name 만 검색)
    public List<Item> findByNameKeyword(String keyword){
        return itemRepository.findByNameKeyword(keyword);
    }

    //Get Item by Brand keyword (brand 만 검색)
    public List<Item> findByBrandKeyword(String keyword){
        return itemRepository.findByBrandKeyword(keyword);
    }
}


```
