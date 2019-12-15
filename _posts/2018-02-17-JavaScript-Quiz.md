---
layout: post
title: Building an online quiz in JavaScript and JQuery
tags: javascript jquery css
---

In this post I will step you through my process of creating the scripts behind [this quiz](https://a-bishop.github.io/javascript-quiz), which tests users on their JavaScript knowledge. 

I recently completed the project for a web scripting class, and I figured I would share some knowledge I gained from the debugging process. You can follow along with the code [here](https://github.com/a-bishop/javascript-quiz/tree/master).

## Building the script
You'll see in my script file that I wrapped all of my code in the following JQuery function. It confirms that the document has fully loaded and the DOM tree is ready to be accessed.

```javascript
$(document).ready(function(){
...
});
```

The next step was to confirm that the browser has the ability to store key-value pairs in its [session storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage). All the rest of our code is again wrapped in this function.

```javascript
if (window.sessionStorage) {
...
}
```

I then wanted to ensure that on the first page load the user is prompted to enter their name. I used the getter method from session storage and compared with null. I also checked for any previously stored highscore value.

```javascript
const username = sessionStorage.getItem("username");
sessionStorage.getItem("highscore");

// if no username, prompt user for name
if (username === null) {
	let name = prompt("Please enter a user name");
	sessionStorage.setItem("username", name);
}
```

I then performed a check on the radio buttons and fired an alert if any questions remained. I used input:radio:checked and compared its length attribute with the total number of questions.

```javascript
// submitting the quiz
$("#submit").click(function() {

	// confirm buttons have all been checked
	if ($("input:radio:checked").length < 5) {
		alert("You need to select answers for all the questions!");
	} else {
		let numCorrect = 0;
	  	let highscore = sessionStorage.getItem("highscore");
	...
    }
 });
```

I was able to use this format again in the next block of code, which checks to see that the id attribute in each of the checked radio buttons contains the same string of text as the correct answers. For this, I also used a regular expression which looks for the pattern "correct" at the end of the id tag (which I set up in my HTML). 

To avoid clever users inspecting my code for the right answer I would have needed to use [obfuscation](http://smallbusiness.chron.com/obfuscate-html-26708.html), but it seemed overkill in this case.

```javascript
// determine if checked buttons are correct
$("input:radio:checked").each(function() {
	if (/correct$/.test($(this).attr("id"))) {
		numCorrect ++;
	}
});
```

Now that I had the number of correct answers, I could update the disappeared JQuery divs that I'd set up in my CSS file for "score" and "results", and display them on the top and bottom of the page. I set them up to fade in for 3 seconds (3000 milliseconds).

```javascript
$("#score").text("You scored " + numCorrect + " out of 5").fadeIn(3000);
$("#results").text("RESULTS: " + sessionStorage.getItem("username") + ", you scored " + numCorrect + " out of 5").fadeIn(3000);
```

The last order of business was to update the high score if the user's current quiz score is greater than the value stored in session storage, and then fade the "fader" div in and out by 2 seconds and 3 seconds, respectively. To accomplish this, I used an if else statement that either sets the text and the session storage or returns the fader to an empty string.

```javascript
if (numCorrect > highscore) {
	$("#fader").text("That's your best score!").fadeIn(2000);
	$("#fader").fadeOut(3000);
	sessionStorage.setItem("highscore", numCorrect);
} else {
	$("#fader").text("");
}
```

And there you have it. Functionality on the [quiz](https://a-bishop.github.io/javascript-quiz/) has been achieved! Give it a go and see how many questions you can get right.
