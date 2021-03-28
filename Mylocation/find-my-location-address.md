```javascript
<div class="container">
    <div class="row">
        <button id="locationBtn" class="w-10 btn btn-dark btn">Search my location</button>
    </div>
</div>

<!--<div id="google_map" style="width:500px;height:500px;"></div>-->

<script>
    const KEY ="AIzaSyBRIdueEX0Qeqz8Wyztxks_rSof_IdTNWM";
    // const LAT = "35.824941";
    // const LNG = "128.464810";

    //경북 = 35.824941 128.464810
    // let url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+LAT+","+LNG+"&key="+KEY;
    let url;
    let city;

    navigator.geolocation.getCurrentPosition(function(pos) {
        var latitude = pos.coords.latitude;
        var longitude = pos.coords.longitude;
        url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+latitude+","+longitude+"&key="+KEY;
        console.log("lat: "+latitude+", lng:"+longitude);
        fetch(url)
            .then(response => response.json())
            .then(data => {
                //console.log(data);
                let parts = data.results[0].formatted_address
                city = parts.split(" ")[1];

                console.log(city+" fetch");
                // parts.forEach(part => {
                //     if(part.types.includes("City")) {
                //         document.body.insertAdjacentHTML(
                //             "beforeend",
                //             '<p>COUNTRY: ${part.long_name}</p>'
                //         )
                //     }
                // })

                // $(document).ready(function(){
            });
        // })
    });
    $("#locationBtn").click(function(){
        // e.preventDefault();
        console.log(city+" ajax");
        let geolocation = {city};
        $.ajax({
            url : "/find-my-weather",
            type : "GET",
            data : geolocation,
            success : function(data) {
                // alert(data);
            },
        })//ajax
    });




</script>
```
