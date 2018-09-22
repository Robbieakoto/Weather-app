<template>

    <div id="index">

            <div class="row">
                <div class="col-lg-3 col-sm-4"></div>
                    <div class="col-lg-6 col-sm-4">
                        <div class="container">
                            <h3><span class="city"></span>-<span class="country"></span></h3>
                            <br><br>
                            <div class="weather text-center">
                                <h2 class="temp"></h2>
                                <div class="iconpic">
                                    <img src="" class='img img-responsive center-block'>
                                </div>
                                <h4 class="weatherDetail"></h4>
                                <div class="toggle" id="celsius">
                            <button type="button" class="btn btn-danger">Temperature</button>                                
                            </div>
                            </div>
                        </div>
                    </div>
                <div class="col-lg-3 col-sm-4"></div>
            </div>

    </div>


</template>

<script>


export default {
  name: 'index',
  
  methods: 
  $(document).ready(function() {
    var fahrenheit, celsius;
    var weatherApiUrl = "https://api.openweathermap.org/data/2.5/weather";
    var apiKey = "dcc079c79880c25d007f9bfdea23eaf0"; //<weather-api-key>
    getLatLong(); //get latitude and longitude

    function getLatLong() {
        $.ajax({
            url: "https://geoip-db.com/json/",
            type: 'GET',
            dataType: 'json',
            success: function(data) {
                var lat = data.latitude;
                var long = data.longitude;
                $('.city').html(data.city);
                $('.country').html(data.country_name);
                weatherApiUrl += "?lat=" + lat + "&lon=" + long + "&APPID=" + apiKey + "&units=metric";
                getWeatherData();
            },
            error: function(err) {
                alert('Oops something went wrong, Please try again.');
                console.log(err);
            }
        });
    }

    //make a request to weather api and give waether and temperature description
    /*weather api start*/
    function getWeatherData() {
        $.ajax({
            url: weatherApiUrl,
            type: 'GET',
            dataType: 'json',
            success: function(data) {
                var temprature = data.main.temp;
                celsius = temprature;
                fahrenheit = celsius * 1.8 + 32;
                var icon = data.weather[0].icon;
                var weatherDetail = data.weather[0].main + ", " + data.weather[0].description;
                $('.weatherDetail').html(weatherDetail); //update weather description in html
                $('.iconpic>img').attr('src', 'http://openweathermap.org/img/w/' + icon + '.png'); //update the icon based on weather
                $('.temp').html(temprature + "&#8451;"); //update the temprature
            },
            error: function(err) {
                alert('Oops something went wrong, Please try again.');
                console.log(err);
            }
        });
    }

    /*weather api end*/

    $('.toggle .btn').click(function() {
        // get id celsius and convert to fahreheit
        if ($('.toggle').attr('id') == 'celsius') {
            $('.temp').html(fahrenheit + "&#8457;");
            $('.toggle').attr('id', 'f');
        } else if ($('.toggle').attr('id') == 'f') {
            //get id fahrenheit and convert to celsius when clicked
            $('.temp').html(celsius + "&#8451;");
            $('.toggle').attr('id', 'celsius');
        }
    });
})

}
  

</script>

<style>

body,html{
    background-image: url("./assets/background.png");
    background-repeat: no-repeat;
    background-size: cover;
}
.container {
  background-color: rgba(236, 240, 241, 0.8);
  padding: 35px 0px;
  margin-top: 50px;
  border-radius: 5px;
}
.weather .temp {
    margin-top: 0px;
}

.weather .iconpic img {
    min-width: 150px;
    min-height: 100px;
}
h3 {
    text-align: center;
}

a {
    text-decoration: underline;
    color: inherit;
}

</style>

