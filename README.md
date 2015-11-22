# fbk-locaiton
# search bands by location
<!DOCTYPE HTML>
<html>
<head>
    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js"></script>
    <link href="//netdna.bootstrapcdn.com/twitter-bootstrap/2.3.1/css/bootstrap-combined.min.css" rel="stylesheet">
    <script src="//netdna.bootstrapcdn.com/twitter-bootstrap/2.3.1/js/bootstrap.min.js"></script>
    <link href="//netdna.bootstrapcdn.com/twitter-bootstrap/2.3.1/css/bootstrap-responsive.min.css" rel="stylesheet">
    <script type="text/javascript" src="../common/get_key_with_callback.js"></script>
<title> Search by location & desc fam</title>
</head>

<body>

<div id="hero" class="hero-unit">
  <div class="container">
    <h1> search location/fam desc</h1>
        <h4> Find popular artists from a particular location, from least known</h4>
        <br>
        <p>

      <div class="container lead span9">
            Enter artist city, state and/or country:
            <input id='locale-input' type="text" class="input-block-level" placeholder="Brooklyn, NY, US">

            <small>
                You can qualify locations with <b>city:</b> <b>region:</b> and <b>country:</b>.  For example 
                <b>city:Brooklyn</b> will find artists from cities called Brooklyn. Find artists from the country 
            </small>

      </div>
      <div class="container lead span12">
            <ul id='results'> </ul>
      </div>

      <div class="container span8">
        <small>
        Powered by <a href="http://echonest.com">The Echo Nest</a>.  See the <a
        href='https://github.com/plamere/en-demos/blob/master/location/artist_location.html'>source</a>.
        </small>
      </div>
  </div>
</div>
</body>

<script type="text/javascript">
jQuery.ajaxSettings.traditional = true; 
var endpoint = 'http://developer.echonest.com/api/v4/'
var apiKey = AXFBOMEM8YBUMGGK7';

function fetchArtistsByLocation(locale) {
    var url = endpoint + 'artist/search';
    $("#results").empty();
    $.getJSON(url, 
        { 
            'api_key' : apiKey,
            'artist_location': locale, 
            'results' : 100,
            'bucket': [ 'artist_location'],  
            'sort': 'familiarity-desc'
        },

        function(data) {

            if (data.response.status.code == 0) {
                var artists = data.response.artists;
                if (artists.length > 0) {
                    for (var i = 0; i < artists.length; i++) {
                        var artist = artists[i];
                        var li = $("<li>");
                        if ('artist_location' in artist) {
                            li.text(artist.name + " from " + artist.artist_location.location);
                            $("#results").append(li);
                        } else {
                            console.log(artist);
                        }
                    }
                } else {
                        $("#results").text("No results");
                }
            } else {
                alert("Trouble getting artists: " + data.response.status.message);
            }
        })
        .error( 
            function(data) {
                alert("query syntax error. Use 'city:', 'region:' and 'country:' qualifiers only");
            }
        );
}

</script>
</html>
