```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src="https://maps.googleapis.com/maps/api/js?libraries=places&callback=initAutocomplete&language=nl&output=json&key=AIzaSyBPy2QU3wtXuZccMWsc40dlkp6qd31xRag" async defer></script>
    <script>
        var autocomplete = null;
        var componentForm = {
        street_number: 'short_name',
        route: 'long_name',
        locality: 'long_name',
        administrative_area_level_2: 'long_name',
        country: 'long_name',
        postal_code: 'short_name'
        };

        function initAutocomplete() {
        var address = document.getElementById('address');
        var options = {
            types: ['address'],
            componentRestrictions: {country: ['usa']}
        }
        var options2 = {
            types: ['address'],
            componentRestrictions: {country: ['uk']}
        }
        var options3 = {
            types: ['address'],
            componentRestrictions: {country: ['kr']}
        }
        ;

        autocomplete = new google.maps.places.Autocomplete(address, options);
        autocomplete.addListener('place_changed', fillInAddress);
        }

        function fillInAddress() {
        // Get the place details from the autocomplete object.
        var place = autocomplete.getPlace();

        for (var component in componentForm) {
            document.getElementById(component).value = '';
            document.getElementById(component).disabled = false;
        }

        // Get each component of the address from the place details
        // and fill the corresponding field on the form.
        for (var i = 0; i < place.address_components.length; i++) {
            var addressType = place.address_components[i].types[0];
            if (componentForm[addressType]) {
            var val = place.address_components[i][componentForm[addressType]];
            document.getElementById(addressType).value = val;
            }
        }
        }
    </script>

<input type="text" id="address">
</body>
</html>
```
