---
layout: post
title: Updating Bootstrap modal windows with back-end data
tags: php mysql bootstrap modal-windows javascript jquery
---

Bootstrap's modal window feature can be implemented by fetching and dynamically updating the content with data from a database. In the case of a LAMP stack, this process involves MySQL queries with PHP.

## Bootstrap modals

Modal windows are a handy way to present important information when an element is clicked. Integrating them in a web app normally requires only a combination of HTML, CSS and JavaScript. However, another layer of complexity is added when fetching data dynamically from a database.

I learned about techniques for working with modal windows when working on an e-commerce website with my classmates earlier this year. The premise for the project was a site where a potential client could sell used vinyl records, and one of the features we implemented was a modal window that displayed additional information about each album when clicked.

## Fetching data and populating the HTML page

Without going too in-depth into our data model, the first step was to fetch data from our database using subqueries on the tables RECORD, RECORD_CATEGORY and GENRE (our client wanted the vinyl records to be viewable by genre).

```php
<?php
if (isset($_GET['genre'])) {
  $genre = $_GET['genre'];
  $query = "select * from RECORD where itemNumber IN
   (SELECT RECORD_itemNumber from RECORD_CATEGORY where GENRE_genreID IN
   (SELECT genreID from GENRE where genre = '$genre'))";
  $result = mysqli_query($link, $query) or die("Error: ".mysqli_error($link));
} else {
  $query = "select * from RECORD";
  $result = mysqli_query($link, $query) or die("Error: ".mysqli_error($link));
}
while ($row = mysqli_fetch_array($result)) {
  echo " html with dynamic data for each album... "
?>
```

Inside the updated album information was a button that would load attributes from the fetched rows. Importantly for the modal window generation, I assigned the `data-toggle` attribute to `modal`, and `data-target` to the ID for the modal window I wanted to target when the user clicks on the button.

```php
<?php ... echo "<button link='" . $row['spotifyLink'] . "' description='" . htmlspecialchars(($row
['description']), ENT_QUOTES) . "' releaseDate='" . $row['RELEASEDATE'] . "' title='" .
htmlspecialchars(($row['albumTitle']), ENT_QUOTES) . "' artist='" . htmlspecialchars(($row['artist']),
ENT_QUOTES) . "' class='more-info btn btn-info' data-toggle='modal' data-target='#myModal'>...";
... ?>
```

## JavaScript event handling

I then passed this data to the modal window through event handlers in my JavaScript file (using the JQuery library). You'll notice the embed link for the Spotify player that I'm concatenating with the link I fetched from our RECORD table, and then assigning to the `src` attribute of the `link` ID in the modal window.

```javascript
$('.more-info').click(function() {
  var record = 'https://embed.spotify.com/?uri=';
  record += $(this).attr('link');
  var description = $(this).attr('description');
  var title = $(this).attr('title');
  var artist = $(this).attr('artist');
  var titleArtist = title + ' - ' + artist;
  var releaseDate = $(this).attr('releaseDate');
  $('#titleArtist').html(titleArtist);
  $('#link').attr('src', record);
  $('#description').html(description);
  $('#releaseDate').html(releaseDate);
});
```

## ...Et voilà

And here's the modal window itself, which as per Bootstrap's recommendation I placed outside of the main content at the end of my page. It establishes a connection to the page through the container div's ID attribute, `myModal`, and loads with data sent to it through the `titleArtist`, `link`, `description` and `releaseDate` IDs.

```html
<div class="modal fade" id="myModal">
  <div class="modal-dialog" style="max-width:400px;">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal">
          &times;
        </button>
        <h4 class="modal-title" id="titleArtist">Info</h4>
      </div>
      <div class="modal-body">
        <span><strong>Release Date: </strong></span>
        <p id="releaseDate"></p>
        <span><strong>Description: </strong></span>
        <p id="description"></p>
        <span><strong>Listen On Spotify: </strong></span>
        <iframe
          id="link"
          src=""
          width="250"
          height="80"
          frameborder="0"
          allowtransparency="true"
          allow="encrypted-media"
        ></iframe>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">
          Close
        </button>
      </div>
    </div>
  </div>
</div>
```

## Summary

Modal windows are a great and intuitive way to emphasize clicked content on a dynamic website. Fetching data from the back-end to populate the modal window with information is an important aspect of integrating a modal window into a data-heavy website such as a LAMP stack e-commerce site. Here is a quick screencast to show you how the functionality worked on our site!

<div
 style="padding-bottom:56.25%; padding-top:10%; position:relative; display:block; width: 100%">
 <video
  width="100%" height="100%"
  controls preload
  style="position:absolute; top:0; left: 0">
  <source src="../videos/micks-licks-more-info.mp4">
  </video>
</div>

I've put the code in a [gist on GitHub](https://gist.github.com/a-bishop/01608bbe9da12248e1416cbbe0b2b11c) if you are interested in viewing it all in one place or tweaking it to your needs.
