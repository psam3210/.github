<br><br>
<figure align="center">
  <img src="on-fire.jpg">
  <figcaption>K.C. Green. <em>Gunshow 648</em>. 2013. <a href="https://gunshowcomic.com/648">↗</a></figcaption>
</figure>
<br><br>

# Shortcuts and Debugging

By nature, programmers tend to be quite lazy. As a result, a lot of mundane, annoying, or otherwise trivial tasks have been streamlined significantly. These shortcuts can greatly increase the speed at which you program and overall just make the experience "nicer" for you.

And of course, a lot of programming involves errors. Getting errors and solving them really just is the process of programming. So when we come across an error, what should we do?

## Shortcuts

In this section, we'll cover both shortcuts in the literal sense, as keyboard combinations for specific functions, and extensions that you can install to streamline additional processes for you.

### Keyboard Shortcuts

This list is obviously not exhaustive as there are hundreds of keyboard shortcuts that you can use and you can even make your own. The ones below are the ones that end up being used the most or offer the most utility.

For keyboard shortcuts, the actual buttons you'd press on your keyboard will be styled in a block like `this` and will be accompanied by information describing what the result is. 

Note that in situations like `ALT/OPTION` and `CTRL/CMD` refer to the same button but `ALT` and `CTRL` are for Windows computers and `OPTION` and `CMD` are for Apple devices.

Also *cursor* refers to the flickering line that shows where you are currently typing.

- `ALT/OPTION` + `LEFT CLICK` = Each left click will put down a new instance of the cursor where you can type from. Useful for writing the same thing in multiple places.

- Highlight/Select a word & `CTRL/CMD` + `D` = Select the next instance of that word. Very useful if you want to rename a variable in multiple places. If you don't have any word selected, this command will select whatever word the cursor is at.

- Select some text & `TAB` = *Increase* the indent of the selected text.
- Select some text & `SHIFT` + `TAB` = *Decrease* the indent of the selected text.

- `CTRL/CMD` + `ENTER` = Insert a new line *below* the cursor position without breaking the current line.

- `SHIFT` + `CTRL/CMD` + `ENTER` = Insert a new line *above* the cursor position without breaking the current line.

- `HOME` = Move cursor to the start of the line.
- `END` = Move cursor to the end of the line. 
- Note that for `HOME` and `END`, sometimes you may not have these keys on your keyboard.

- `CTRL/CMD` + `/` = Toggle comment marks on selected text

- `CTRL/CMD` + `X` = This cuts whatever text you selected and I usually pretty commonly used but just to point out that using this *also copies* whatever you cut so you can paste it somewhere else.

- Double `LEFT CLICK` = Selects whatever *word* you double clicked.

- Triple `LEFT CLICK` = Selects whatever *line* you triple clicked.
- `CTRL/CMD` + `L` = Selects the entire line that the cursor is on

- `ALT/OPTION` + `UP ARROW` or `DOWN ARROW` = Takes the selected chunk of text and literally moves up and down.

### Programmed Shortcuts (or Extensions)

Whereas before we were using keyboard shortcuts to streamline some processes, this section is looking at small bits of software that people have developed to make certain things easier. These *extensions* are typically added to whichever code editor you are using. If you are using VSCode, you can find extensions on the left-hand sign indicated by an icon of 4 blocks.

As with keyboard shortcuts, there are hundreds of extensions that you can download. Of course, many of them are trivial but there are some incredibly useful ones as well. As before, this list is just meant to highlight a few extensions with high utility.

[**Live Server**](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) by Ritwick Dey:

  - This is an extension that you should already have but I'll put it here anyways. Live Server just allows you to open your web projects on an actual server on your local computer. This allows for much easier development as the website will auto-refresh when you save changes in your code editor and your website acts like an actual website rather than a static file.

[**HTML Boilerplate**](https://marketplace.visualstudio.com/items?itemName=sidthesloth.html5-boilerplate) by sidthesloth:

  - In general, boilerplate refers to super basic and standard code that is just repeated across various websites. When setting up an `index.html`, it can be a little annoying to write out all the foundational HTML to get the site working. This extension remedies that be just creating a basic template to get you started that you can then easily input into any HTML file.

[**Prettier**](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) by Prettier:

  - Prettier is just an extension that will format your code to be a lot more legible when you activate it. This is great when you have files with a lot of layered complexity and you want it to be sorted a bit nicer so you can read it more clearly.

### Themes

This will be more of a side note but one of the crucial aspects of your code editor is the theme that you use. Themes can be quite important when it comes to debugging due to their *syntax highlighting* which is the way they stylize and color different aspects of your code. 

Some themes have very rich syntax highlighting whereas others are more simple. Developers may have preference for a dark theme or a light theme (although light theme devs are not to be trusted). Ultimately choose a theme that you like and works in a way that helps you.

Some popular (not default) themes include:

- [One Dark Pro](https://marketplace.visualstudio.com/items?itemName=zhuangtongfa.Material-theme)

- [Dracula](https://marketplace.visualstudio.com/items?itemName=dracula-theme.theme-dracula)

- [GitHub Theme](https://marketplace.visualstudio.com/items?itemName=GitHub.github-vscode-theme)

- [One Monokai](https://marketplace.visualstudio.com/items?itemName=azemoh.one-monokai)

### Flux

I wasn't really sure where to put this but some software I *extremely* recommend is the blue-light filter for your screen [flux](https://justgetflux.com/). It's free and pretty basic in that all it does is apply a orange/red tint to your screen to help with blue-light eyestrain.

This is a *lifesaver* for chronic screen use (especially if you're a night owl programmer) and is just easy on your eyes. Of course, you can disable it for set periods of time if you're doing color-sensitive work.


## Debugging

If you can remember from our earlier lecture, the word "bug" originated from when computers used vacuum tubes and actual insects would get stuck inside them, causing the computer to malfunction. 

Nowadays, bugs refer to errors in our code that either cause it to not function as we expect/want or not even function at all. Debugging, is the process of solving those errors.

Frankly, debugging is a skill that you really only get good at as you do more of it. The more you expose yourself to code and the potential places for errors, the better you are at recognizing them.

Now, there are some good methods to know when trying to debug that make the process much easier, which we'll get into here.

### Console

You must've used it a ton already, but the Javascript `console` is the premier tool for developers to debug their code. The main way we use it is by calling its `log()` method and printing out messages.

```js
  console.log('hello');
  // This will write the message 'hello' to our console

  let num1 = 4;
  let num2 = 10;

  console.log(num1);
  // This will write the message 4 to our console

  console.log(num1 + num2);
  // This will write the message 14 to our console
```

`log()` is the primary way that we debug our programs but know how to utilize it properly is key!

Let's try looking at simple program:

```js
  let num1 = 4;
  let num2 = 10;
  console.log(num1, num2);
  // This will write the message 4, 10 to our console
```

Now, when we go to look at our console, we'll only see the output `4`, `10` and not know the original two variables where they came from. How might we do this better? Complex strings!

```js
  console.log(`num1 = ${num1}`, `num2 = ${num2}`);
  // or, alternatively:
  console.log('num1 =' + num1, 'num2 = ' + num2);
  // This will write the message 'num1 = 4, num2 = 10' to our console

  console.log(num1 + num2);
  // This will write the message 14 to our console
```

With this implementation, we have a much better understanding of our `console` output since we properly labeled our variables. Of course, this is a little much for our current program but you can see how this becomes incredibly important once we start getting more and more complex programs.

```js
  let apple = {
    type: 'gala',
    weight: '163g',
    organic: true
  }
  console.log(`apple = ${apple}`);
  // This will write the full apple object to our console: 
  // 'apple = {type: 'gala', weight: '163g', organic: true}'

  for (let i = 0; i < 4; i++) {
    console.log(i);
  }
  // This will log i to the console every time the loop is ran:
  // 0
  // 1
  // 2
  // 3
  // This is good way to check that your for loops are running as many times as you expect them to
```

Something that is also nice is `console.table()`. This works only for objects and arrays but `console.table()` will take whatever it is given and format that data into a nicer looking table structure. This is great when you're working with objects with a lot of properties and you want to be able to see all of them clearly:

```js
  const employees = [
    { id: 1, name: 'John', department: 'HR', salary: 50000 },
    { id: 2, name: 'Jane', department: 'Engineering', salary: 60000 },
    { id: 3, name: 'Bob', department: 'Marketing', salary: 45000 },
    { id: 4, name: 'Alice', department: 'Finance', salary: 55000 },
  ];

  console.table(employees);
```

Another useful feature is that you're also able to `console.log` references to HTML elements. Hovering over the element in the console will also highlight that HTML element on the page itself:

```js
  h1El = document.createElement('h1');
  h1.classList.add('header');
  h1El.style.width = '40px';
  h1El.style.height = '40px';    
  document.body.appendChild(h1El);
  console.log(h1El);
```

### What To Do If You Don't Know What To Do

With the previous situation, we knew at which points we wanted our `console.log()` to be and could reasonably point out where a bug might occur. But what happens when we're doing something new and don't know where errors can occur?

Let's say we have this program here where we have a variable `string` that we want to turn into an array:

```js
  let string = 'dog, cat, turtle, fish, bird';
  // We know that we can use split() to separate a string into an array
  string.split()
  
  //Now let's test our code to see if we got what we want 
  for (let i = 0; i < string.length; i++) {
    let item = string[i];
    console.log(`item  = ${item}`);
  }
```

Well for some reason, when we do this we just get a single output of our original string. What could we do to solve our problem? Well, an initial idea would be to just Google 'Turn a string into an array javascript'. From there, we can choose between:

[**MDN Web Docs**](https://developer.mozilla.org/en-US/):
  MDN Web Docs is a website that has generic information about nearly every possible thing when it comes to developing for the web. They have information on HTML tags, CSS properties, Javascript functions, and everything in between. 
  
  Of course, this information is very general and may not apply your specific use case. If you're running into an error where x is not equal to y, MDN Web Docs can explain what that means in a general sense but otherwise lacks context to individual situations.

[**StackOverflow**](https://stackoverflow.com/):
  StackOverflow is a great website as its main premise is developers asking questions and getting them answered by other developers. StackOverflow is great as people are quite good at answering questions and, more often than not, people have already asked your exact question.
  
  Unlike MDN Web Docs, StackOverflow is more likely to have a specific/unique context where something is not working that matches up with yours. Also, StackOverflow is for all programming languages whereas MDN Web Docs is more geared towards web development.

[**ChatGPT**](https://chat.openai.com/):
  Fortunately (or unfortunately), we are in the future and we now have AI language models that are quite good at helping us code and debug our code. ChatGPT is great in that it offers more human-understandable language so that you're not bogged down by super-heavy technical terms. ChatGPT is also quite good at understand the specific context for your situation: You can paste in your error messages or even your entire code! 

  However, there are situations where ChatGPT may not be the best programmer. Using ChatGPT and trusting it completely can be quite tricky as the model is statistical, and there are just situations where it might not give you an entire optimal solution. In this type of situation, you really have to know your fundamentals to be sure that ChatGPT isn't giving you bad code.

[**Phind**](https://www.phind.com/):
  Phind is another AI tool that is sort of like a combination of the strengths of StackOverflow and ChatGPT. Phind, like ChatGPT, is a generic tool not specific to coding but what it does is offer summaries of search results. What's nice about it is that Phind links to all of the sources that it is summarizing in case you want to double check the code. 

  This means that you get the nice, human-readability of ChatGPT (and free access to use GPT-4 😁) while being able to be a bit more confident about the source of the code as with StackOverflow. The links also enable you to do some further research in case you don't quite get the answer you're looking for or you're not quite sure what you're looking up.

So after that, we now know that we were supposed to add an indication of where to split our string, which in our case is at the commas.

```js
  let string = 'dog, cat, turtle, fish, bird';
  // We know that we can use split() to separate a string into an array
  string.split(',');
  
  //Now let's test our code to see if we got what we want 
  for (let i = 0; i < string.length; i++) {
    let item = string[i];
    console.log(`item  = ${item}`);
  }
```

And there we go, we've debugged our program! Of course, typical debugging is almost never this easy so tread calmly and carefully and slowly you'll get the hang of it! And of course, you can always ask your friends for help, the developer community is always very friendly!