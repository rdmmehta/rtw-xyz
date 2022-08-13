---
layout: post
title:  "Add reCAPTCHA to default Django admin login form"
date:   2021-10-06 20:41:00 +0700
categories: [python, django, security]
---

<style>
    /* css here */

{
  border: 1px dotted black;
}

p.question {
  font-family: Arial, sans-serif;
	  font-size:20px;
  color: #2E2E2E;
  margin-bottom:0px;
}

h2.quizHeader {
  font-family: Arial, sans-serif;
  font-weight:normal;
  font-size:25px;
  line-height: 27px;
  margin: 24px 0 12px 0;
  padding: 0 0 4px 0;
  border-bottom: 1px solid #a2a2a2;
}

h2.quizScore{
  font-family: Arial, sans-serif;
  font-size:25px;
}

div.quizAnswers{
  font-family: Arial, sans-serif;
  font-size:16px;
  color: #424242;
  padding: 4px 0 4px 0;
}

label {
  font-family: Arial, sans-serif;
  font-size:14px;
  color: #424242;
  vertical-align:top;
}

input.answer[type="radio"] {
  margin-bottom: 10px;
}

input.quizSubmit[type="submit"] {
  -webkit-background-clip: border-box;
  -webkit-background-origin: padding-box;
  -webkit-background-size: auto;
  -webkit-transition-delay: 0s, 0s;
  -webkit-transition-duration: 0.2s, 0.2s;
  -webkit-transition-property: color, background-color;
  -webkit-transition-timing-function: ease, ease;
  box-shadow: rgba(0, 0, 0, 0.498039) 0px 0px 5px 0px;
  color: #ffffff;
  background-color: #c30b0a;
  margin: 0;
  border: 0;
  outline: 0;
  text-transform:uppercase;
  height:35px;
  width:85px;
  border: 1px solid #5E5E5E;
  border-radius:5px;
  
 }

input.quizSubmit[type="submit"]:hover {
  color: #ffffff;
  background: #680f11;
  text-decoration: none;
}

table {
  background-color: #F2F2F2;
	  border:1px solid #BDBDBD;
  border-radius:5px;
  padding:10px;
  padding-left:25px;
  box-shadow: rgba(0, 0, 0, 0.498039) 0px 0px 1px 0px;
}

th {
  
}

tr {
  
}

td {

}

.submitter {
	  width:85px;
}

.hide {
	  display:none;
}


/*SFS light red = #c30b0a;
SFS dark red = #9f2026; */


</style>
<script>
    // scripts here:

	function submitQuiz() {
		console.log('submitted');

	// get each answer score
		function answerScore (qName) {
			var radiosNo = document.getElementsByName(qName);

			for (var i = 0, length = radiosNo.length; i < length; i++) {
   				if (radiosNo[i].checked) {
			// do something with radiosNo
					var answerValue = Number(radiosNo[i].value);
				}
			}
			// change NaNs to zero
			if (isNaN(answerValue)) {
				answerValue = 0;
			}
			return answerValue;
		}

	// calc score with answerScore function
		var calcScore = (answerScore('q1') + answerScore('q2') + answerScore('q3') + answerScore('q4'));
		console.log("CalcScore: " + calcScore); // it works!

	// function to return correct answer string
		function correctAnswer (correctStringNo, qNumber) {
			console.log("qNumber: " + qNumber);  // logs 1,2,3,4 after called below
			return ("The correct answer for question #" + qNumber + ": &nbsp;<strong>" +
				(document.getElementById(correctStringNo).innerHTML) + "</strong>");
			}

	// print correct answers only if wrong (calls correctAnswer function)
		if (answerScore('q1') === 0) {
			document.getElementById('correctAnswer1').innerHTML = correctAnswer('correctString1', 1);
		}
		if (answerScore('q2') === 0) {
			document.getElementById('correctAnswer2').innerHTML = correctAnswer('correctString2', 2);
		}
		if (answerScore('q3') === 0) {
			document.getElementById('correctAnswer3').innerHTML = correctAnswer('correctString3', 3);
		}
		if (answerScore('q4') === 0) {
			document.getElementById('correctAnswer4').innerHTML = correctAnswer('correctString4', 4);
		}

	// calculate "possible score" integer
		var questionCountArray = document.getElementsByClassName('question');

		var questionCounter = 0;
		for (var i = 0, length = questionCountArray.length; i < length; i++) {
			questionCounter++;
		}

	// show score as "score/possible score"
		var showScore = "Your Score: " + calcScore +"/" + questionCounter;
	// if 4/4, "perfect score!"
		if (calcScore === questionCounter) {
			showScore = showScore + "&nbsp; <strong>Perfect Score!</strong>"
		};
		document.getElementById('userScore').innerHTML = showScore;
	}

$(document).ready(function() {

	$('#submitButton').click(function() {
		$(this).addClass('hide');
	});

});
    </script>
    
      <h2 class="quizHeader">Take a Quiz!</h2>
  
	<table style="width:583px">
	<tr>
	  <td>
	  	<div>
	  		<p class="question">1. Quiz question #1</p>

	    	<ul>
	    	<input class="answer" type="radio" name="q1" value="1">
	        <label id="correctString1">correct answer 1</label>
	    	<br>
	        <input class="answer" type="radio" name="q1" value="0">
	        <label>wrong answer</label>
	        <br>
	        <input class="answer" type="radio" name="q1" value="0">
	        <label>wrong answer</label>
	        <br>
	        <input class="answer" type="radio" name="q1" value="0">
	        <label>wrong answer</label>
    		</ul>
	  	</div>
	  </td>
	  <td>
	  	<div>
	  		<p class="question">2. Quiz question #2</p>

	    	<ul>
	        <input class="answer" type="radio" name="q2" value="0">
	        <label>wrong answer</label>
	        <br>
	        <input class="answer" type="radio" name="q2" value="1">
	        <label id="correctString2">correct answer 2</label>
	        <br>
	        <input class="answer" type="radio" name="q2" value="0">
	        <label>wrong answer</label>
	        <br>
	        <input class="answer" type="radio" name="q2" value="0">
	        <label>wrong answer</label>
	    	</ul>
	  	</div>
	  </td>
	</tr>
	<tr>
	  <td>
	  	<div>
	  		<p class="question">3. Quiz question #3</p>

	    	<ul>
	        <input class="answer" type="radio" name="q3" value="0">
	        <label>wrong answer</label>
	        <br>
	        <input class="answer" type="radio" name="q3" value="1">
	        <label id="correctString3">correct answer 3</label>
	        <br>
	        <input class="answer" type="radio" name="q3" value="0">
	        <label>wrong answer</label>
	        <br>
	        <input class="answer" type="radio" name="q3" value="0">
	        <label>wrong answer</label>
	    	</ul>
		</div>
	  </td>
	  <td>
	  	<div>
	  		<p class="question">4. Quiz question #4</p>

	    	<ul>
	        <input class="answer" type="radio" name="q4" value="0">
	        <label>wrong answer</label>
	        <br>
	        <input class="answer" type="radio" name="q4" value="1">
	        <label id="correctString4">correct answer 4</label>
	        <br>
	        <input class="answer" type="radio" name="q4" value="0">
	        <label>wrong answer</label>
	        <br>
	        <input class="answer" type="radio" name="q4" value="0">
	        <label>wrong answer</label>
    		</ul>
	  	</div>
	  </td>
	</tr>
	</table>
<br/>
  <div class="submitter">
          <input class="quizSubmit" id="submitButton" onClick="submitQuiz()"
          type="submit" value="Submit" />
    </div>
  
<!--show only wrong answers on submit-->
    <div class="quizAnswers" id="correctAnswer1"></div>
	    <div class="quizAnswers" id="correctAnswer2"></div>
    <div class="quizAnswers" id="correctAnswer3"></div>
    <div class="quizAnswers" id="correctAnswer4"></div>

<!--show score upon submit-->
    <div>
    	<h2 class="quizScore" id="userScore"></h2>
    </div>
	</div>


Previously makesure you already install the [`django-recaptcha`](https://pypi.org/project/django-recaptcha/),
don't miss also to [Sign up for reCAPTCHA](https://www.google.com/recaptcha/about/)

```
pip install django-recaptcha
```

Add `'captcha'` to your `INSTALLED_APPS` setting.

```python
INSTALLED_APPS = [
    ...,
    'captcha',
    ...
]
```

Add the Google reCAPTCHA keys generated into your Django settings with `RECAPTCHA_PUBLIC_KEY` and `RECAPTCHA_PRIVATE_KEY`.

```python
RECAPTCHA_PUBLIC_KEY = 'MyRecaptchaKey123'
RECAPTCHA_PRIVATE_KEY = 'MyRecaptchaPrivateKey456'
```

Then modify the default authentication form with add new captcha field, in your `myapp/forms.py`:

```python
from django.conf import settings
from django.contrib.auth.forms import AuthenticationForm

from captcha.fields import ReCaptchaField
from captcha.widgets import ReCaptchaV2Checkbox


class AuthAdminForm(AuthenticationForm):

    if not settings.DEBUG:
        captcha = ReCaptchaField(widget=ReCaptchaV2Checkbox(
            attrs={
                'data-theme': 'light',
                'data-size': 'normal',
                # 'style': ('transform:scale(1.057);-webkit-transform:scale(1.057);'
                #           'transform-origin:0 0;-webkit-transform-origin:0 0;')
            }
        ))
```

Then in your `myproject/urls.py`;


```python
from django.contrib import admin
from django.urls import include, path

from myapp.forms import AuthAdminForm

# modify the default admin login form
# with add reCAPTCHA feature to fix bruteforce issue.
admin.autodiscover()
admin.site.login_form = AuthAdminForm
admin.site.login_template = 'account/admin/login.html'

urlpatterns = [
    path('admin/', admin.site.urls),
    ...
]
```

Also don't miss to add the captcha field into template `templates/account/admin/login.html`;

<iframe width="100%" height="400" src="//jsfiddle.net/agaust/ja21bugn/2/embedded/html/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>
