Amber-Tutorial
==============

Amber Tutorial

Mainly to install Amber you need to follow http://docs.amber-lang.net/
And all this tutorial is following the guidlines in the tutorial them https://github.com/amber-smalltalk/amber/wiki/The-counter-example

After you install amber run this:

    mkdir /path/to/project
    cd /path/to/project
    amber init
    amber serve


for example:
    
     mkdir myapp
     cd myapp
     amber init
     amber serve
 

now, make sure that the link is given to you afeter amber serve is working if not go back to installing amber and go through the installation again.
open the link a pop-up will appear, if not make sure that your browser allows pop-ups then open Helios-IDE

now open class browser, look at packages column (yes, the one on the lefthanside) go down till you find myapp package.
Then, go to the adjacent column of classes you will see myapp as well now press on it and look down on the left you will see a workspace with something written:
        
        Object subclass: #Myapp
	instanceVariableNames: ''
	package: 'Myapp'

Okay, now to make our own Tutorials package and create a Counter:
      
        (simply delete the currently written code and add this or something of your own as you like)


        Widget subclass: #TCounter
	instanceVariableNames: 'count header'
	package: 'Tutorials'

(well I didn't just call it Counter is that Counter already exists in the examples so to prevent confusing conflicts I used 'TCounter' look up at the code to understand)Then, press saveit at the bottom , now if you will see that Tutorials is red while the rest are kind of brownish color. Well, that is because we didnot commit the package yet don't worry we will commit it now. Press on the Tutorials package (yes the red one), now got to the top tab of the packages, look a bit to the left you will see an arrow pointing downwards press om it then press commit package (or if you would like use ctrl+space+k). You should receive "commit sucessfull". If not either you entered an incorrect code then I will advise you to use my examples or check your own errors OR you loaded the page twice and didnt reopen helios that also causes errors I face it alot at the beginning. Okay, if we refresh the page and reopen helios you will see that the package 'Tutorials' is GONE! (WHAT?!!).First check your /project/path and open the src you should find 'Tutorials.st' and another one 'Tutorials.js', if they are not their, you should go back to the installation and re-follow the tutorial. But if they are there do the following open the index.html in a editor (it should look like this): 



	<!DOCTYPE html>
	<html>

  		<head>
    			<title>Myapp</title>
    			<meta http-equiv="content-type" content="text/html; charset=utf-8" />
    			<meta name="author" content="Author" />
			<script type='text/javascript' src='the.js'></script>
  		</head>

  		<body>
  			<p>Hi, Author! Welcome to Amber project: "Myapp".</p>
  			<p>This is the place for your application's HTML. After getting familiar with Amber,just remove this welcome contents from index.html and replace it with your own.</p>
  			<button id="amber-with">Hello from TagBrush >> with:</button>
  			<button id="jquery-append">Hello from jQuery append</button>
  			<ol id="output-list"></ol>
  			<script type='text/javascript'>
      			require(['app'], function (amber) {
        		 amber.initialize({
            			'transport.defaultAmdNamespace': "amber-myapp"
        		 });
          
          		 require(["amber-ide-starter-dialog"], function (dlg) { dlg.start(); });
        		 amber.globals.Myapp._start();
        		});
  			</script>
  		</body>

	</html>

Well do you see the 
	require(['app'], function (amber)
change it to
	require(['app', 'amber-myapp/Tutorials'], function (amber)
Now if you reload helios you will find the package. (Wow! we have done alot till now right?!)


okay lets press on the 'Tutorials' pachage and on the 'TCounter' Class, now look directly under the classes column you will see two things: 1- instance  2-class  (they are beside the Doc checkbox)

well, press on 'instance' and then the rightside column "Methods" now look again at the bottom left workspace, you should see:

	messageSelectorAndArgumentNames
		"comment stating purpose of message"

		| temporary variable names |
		statements

Now add this,The first method is used to initialize the component with the default state, in this case we set the counter to 0:

	initialize
    		super initialize.
    		count := 0

The method used for rendering a widget is #renderOn:. It takes an instance of HTMLCanvas as parameter.The header h1 is kept as an instance variable, so when the count value changes, we can update its contents accordingly.

	renderOn: html
	 	header := html h1 
        		with: count asString;
        		yourself.
    	html button
        	with: '++';
        	onClick: [self increase].
    	html button
        	with: '--';
        	onClick: [self decrease]

The counter is almost ready. All we need now is to implement the two action methods #increase and #decrease to change the state of our counter and update its header.

	increase
    		count := count + 1.
    		header contents: [:html | html with: count asString]

	decrease
    		count := count - 1.
    		header contents: [:html | html with: count asString]

Okay saveit and commit package now press on class instead of instance and add this to a method

run

	TCounter new appendToJQuery: 'body' asJQuery

Okay, save and commit package after that you can catgorize your 'instance' and 'class' method if you want to (look at counter example for ideas).Go to the workspace (no not the one on bottom left press ctrl+space open and press workspace). Now if we write:

    TCounter run
Then press Doit. Tada! you got your counter on your HTML page. But do you have to go to the workspace everytime you want to run the counter well till now yes, but we can make it load automatically by opening theindex.html, okay lets add something:

(it should look like this before edit)
        
          require(["amber-ide-starter-dialog"], function (dlg) { dlg.start(); });
          amber.globals.Myapp._start();
        });

(after edit its like this)
          
          require(["amber-ide-starter-dialog"], function (dlg) { dlg.start(); });
          amber.globals.Myapp._start();
          amber.globals.TCounter._run();
        });

Now save and run open the link from amber serve again it should load automatically.But why did I add this line right well its like this:

     amber.globals.Yourclass._method();

Remember if you have any problems goto https://github.com/amber-smalltalk/amber open an issue and you will find the help you need 

