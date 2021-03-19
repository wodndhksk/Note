```java
@Controller
@Slf4j
@RequiredArgsConstructor
public class MainController {

    private final ItemService itemService;
  
    @GetMapping("/") // root context가 들어오면 index page를 보여준다.
    public String index(@CurrentUser Member member, Model model, String keyword, String searchType){

        if(member != null){
            model.addAttribute("member", member);
        }

        model.addAttribute("title", "Soir.");

        //common_fragments 에서 SearchType 받아서 처리 (검색)
        if(keyword != null){
            if(searchType.equals("item_all"))
                model.addAttribute("clothsList", itemService.findByAllKeyword(keyword));
            else if(searchType.equals("item_name"))
                model.addAttribute("clothsList", itemService.findByNameKeyword(keyword));
            else if(searchType.equals("brand_name"))
                model.addAttribute("clothsList", itemService.findByBrandKeyword(keyword));
        }
        else{
            model.addAttribute("clothsList", itemService.getItemList());

        }
        return "/view/index";
    }

```
