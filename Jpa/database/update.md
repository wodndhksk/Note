## Controller
```java
 @PostMapping("/modify")
    public String modifyAndUpdate(ContentsDto contentsDto, Long idx){
        contentsService.updateContents(contentsDto, idx);
        return "redirect:/";
    }
```

## Service
```java
public void updateContents(ContentsDto contentsDto, Long idx){
        Contents contents = Contents.builder()
                .id(idx)
                .title(contentsDto.getInputTitle())
                .description(contentsDto.getInputDescription())
                .thumbnailImageUrl(contentsDto.getInputImageUrl())
                .build();
        contentsRepository.updateContents(contents);
    }
```

## Repository
```java
 @Transactional
    @Modifying
    @Query("UPDATE Contents set title = :#{#paramContent.title}, description=:#{#paramContent.description}, thumbnailImageUrl=:#{#paramContent.thumbnailImageUrl} WHERE id = :#{#paramContent.id}")
    void updateContents(@Param("paramContent") Contents contents);
```

### HTML
```html
<div class="container">
        <div class="row">
            <h1>dd</h1>
            <form class="row g-3" method="post" th:action="@{/contents/modify}" name="theUpdateForm">
                <div class="col-md-12" >
                    <label for="inputTitle" class="form-label title-reponse text-red">*Title</label>
                    <input type="title" class="form-control justify-content-center" th:name="inputTitle" th:value="${content.title}" id="inputTitle">
                </div>
                <div class="col-md-12" >
                    <label for="inputDescription" class="form-label title-reponse text-red">*Description</label>
                    <input type="description" class="form-control justify-content-center" th:name="inputDescription" th:value="${content.description}" id="inputDescription">
                </div>
                <div class="col-md-12" >
                    <label for="inputImageUrl" class="form-label">ImageUrl</label>
                    <input type="imageUrl" class="form-control justify-content-center" th:name="inputImageUrl" th:value="${content.thumbnailImageUrl}" id="inputImageUrl">
                </div>
                <div style="display: none">
                    <input type="idx" th:name="idx" th:value="${content.id}">
                </div>
                <div class="col-12">
                    <button type="submit" class="btn btn-primary" id="uploadBtn">Submit</button>
                </div>
            </form>
        </div>
    </div>
```
