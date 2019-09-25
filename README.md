# VidjetWeather
test task
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta http-equiv="content-type" content="text/html" charset="utf-8" />
    <script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js"></script>
</head>
<body>
    <table id="main">
        <tr>
            <td colspan="2"><label id="TownCaption" style="font-size:24pt;"></label></td>
        </tr>
        <tr><td colspan="2"><hr /></td></tr>
        <tr>
            <td style="padding-right:10px;">
                <span style="font-size:30pt;">
                    <span id="HeaderTemp"></span><sup style="font-size:20pt;">°C</sup>
                </span>
            </td>
            <td class="img_block">
                <img id="WeatherImage" src="./image/undefined.jpg" style="width:100%;height:100%;" />
            </td>
        </tr>
        <tr>
            <td colspan="2" style="padding-top:10px;">
                <div>&#128167 Влажность: <span id="HeaderHumidity"></span> %</div>
                <div>&#19960 Скорость верта: <span id="HeaderWind"></span> м/с</div>
                <div>Давление: <span id="HeaderPressure"></span> мм. рт. ст.</div>
            </td>
        </tr>
    </table>
    <div>
        <p>Выберите город</p>
        <select id="City" onchange="changeCity();">
            <option value="Moscow,ru">Москва</option>
            <option value="Saint Petersburg,ru">Санкт-Петербург</option>
            <option value="Samara,ru">Самара</option>
            <option value="Volgograd,ru">Волгоград</option>
            <option value="Vladivostok,ru">Владивосток</option>
            <option value="Orel,ru">Орёл</option>
        </select>
        <div style="margin-top:20px;">
            <label id="Output"></label>
        </div>
    </div>

    <style>
        #main {
            display: table;
            background: white; /* Цвет фона */
            border: 2px solid grey;
            padding: 40px;
            }

            .img_block {
            display: inline-block;
            max-width: 184px; /*максимальная ширина*/
            max-height: 157px; /*max высота картинки */
        }
    </style>
    <script>
        $(document).ready(() => {
            changeCity();
        });

        function changeCity() {
            let selected = $("#City option:selected");
            $("#TownCaption").text(selected.text());
            let city = selected.val();
            $.ajax({
                method: "POST",
                url: `http://openweathermap.org/data/2.5/weather/?q=${city}&appid=b6907d289e10d714a6e88b30761fae22`,
                async: false,
                success: function (result) {
                    $("#HeaderPressure").text(result.main.pressure);
                    $("#HeaderHumidity").text(result.main.humidity);
                    $("#HeaderWind").text(result.wind.speed);
                    Temperature(result.main.temp);
                    weatherImage(result.weather[0].icon);
                    //
                    $("#Output").text(JSON.stringify(result));
                },
                fail: function () {
                    resetWeather();
                }
            });
        }

        function Temperature(temp) {
            let result = "";
            let rounded = Math.round(temp);
            if (+rounded> 0) result = "+";
            result += rounded;
            $("#HeaderTemp").text(result);
        }

        function weatherImage(img) {
            $("#WeatherImage").attr("src", `./image/${img}.gif`);
        }

        function resetWeather() {
            $("#HeaderPressure").text("");
            $("#HeaderHumidity").text("");
            $("#HeaderWind").text("");
            $("#HeaderTemp").text("");
            weatherImage("undefined");
        }
    </script>
</body>
</html>
<!--appid=b6907d289e10d714a6e88b30761fae22-->
<!--
JSON api.openweathermap.org/data/2.5/weather?q=London
XML api.openweathermap.org/data/2.5/weather?q=London&mode=xml
HTML api.openweathermap.org/data/2.5/weather?q=London&mode=html
-->
