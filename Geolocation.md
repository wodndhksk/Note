```javascript
<!DOCTYPE html>
<html lang="ko"
      xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="layout" th:with="title='Hello!'">
<div layout:fragment="content">
<div class="container">
    <h1>location</h1>
    <div class="row">
        <button id="locationBtn" class="w-10 btn btn-dark btn" type="submit">Search my location</button>
    </div>
</div>

<!--<div id="google_map" style="width:500px;height:500px;"></div>-->

<script>
    const KEY ="AIzaSyBRIdueEX0Qeqz8Wyztxks_rSof_IdTNWM";
    const LAT = "37.5347225"//navigator.geolocation.getCurrentPosition(position.coords.latitude);
    const LNG = "126.9753714"//navigator.geolocation.getCurrentPosition(position.coords.longitude);
    let url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+LAT+","+LNG+"&key="+KEY;
    let city;

    if(navigator.geolocation){
        navigator.geolocation.getCurrentPosition(function (position){
           const latitude = position.coords.latitude;
           const longitude = position.coords.longitude;
           var pos={
               lat: position.coords.latitude,
               lng: position.coords.longitude
           }
        })
    }

    fetch(url)
    .then(response => response.json())
    .then(data => {
        //console.log(data);
        let parts = data.results[10].address_components[1];
        city = parts.long_name;

        console.log(city);
        // parts.forEach(part => {
        //     if(part.types.includes("City")) {
        //         document.body.insertAdjacentHTML(
        //             "beforeend",
        //             '<p>COUNTRY: ${part.long_name}</p>'
        //         )
        //     }
        // })

        // $(document).ready(function(){
            $("#locationBtn").click(function(){
               // e.preventDefault();
                console.log(city);
                let geolocation = city;
                $.ajax({
                    url : "/find-my-location",
                    type : "GET",
                    data : geolocation,
                    success : function(data) {
                        alert(data);
                    },
                })//ajax
            });
        });

    // })

</script>
</div>
</html>
```
