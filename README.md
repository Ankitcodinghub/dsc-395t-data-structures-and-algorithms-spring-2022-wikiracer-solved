# dsc-395t-data-structures-and-algorithms-spring-2022-wikiracer-solved



**<span style='color:red'>TO GET THIS SOLUTION VISIT:</span>** https://www.ankitcodinghub.com/product/dsc-395t-data-structures-and-algorithms-spring-2022-wikiracer-solved/

In this assignment, you will implement different types of graph searches that will run on Wikipedia. Speciﬁcally,

Wikipedia pages are the graph nodes and links to other pages are the edges.

1 Wikipedia, Wikiracer, and Graph Algorithms

Wikipedia is a giant online encyclopedia that anyone can edit (with approval), available at wikipedia.org. Among many other interesting properties, its size makes it particularly hard to digest. As such, a wiki racing game has been created that uses Wikipedia’s large graph as the basis for games. The goal is get from one speciﬁed Wikipedia page (the start node) to another (the goal node) as fast as possible (or perhaps in as few clicks as possible). You can ﬁnd variations of this game online by searching “Wikiracer.” We recommend that you play the game a couple of times ﬁrst before beginning this assignment. Here is one implementation that we recommend playing: https: //dlab.epfl.ch/wikispeedia/play/.

Before beginning this assignment, you should be familiar with the different types of graph searches that we dis-

cussed in lecture, speciﬁcally, breadth-ﬁrst search (bfs), depth-ﬁrst search (dfs), and Dijkstra’s algorithm.

2 Your Assignment

2.1 Parser

This assignment has a few parts. First, we ask you to build a Wikipedia page parser. In other words, once we’ve downloaded the HTML markup for a Wikipedia page, we need to read the HTML code and ﬁnd all of the page’s neighbors (links to other Wikipedia pages). We provide the code to download a page’s HTML code, but we ask you to parse it inside the get_links_in_page function inside of py_wikiracer/wikiracer.py.

HTML code is organized into different cascading tags, and it is the building block of all websites. For instance,

consider the following HTML markup:

&lt;html&gt;

&lt;head&gt;

&lt;script href=”…”&gt;&lt;/script&gt;

&lt;/head&gt;

&lt;body&gt;

&lt;p&gt;This is a paragraph!&lt;/p&gt;

&lt;p&gt;I can put any text I want, including &lt;a href=”/wiki/Science”&gt;a link&lt;/a&gt;.&lt;/p&gt;

&lt;/body&gt;

&lt;/html&gt;

When Google Chrome or Firefox encounter this HTML segment, they will interpret the HTML commands and cre- ate a webpage with two paragraphs. The second one will have a link over the text “a link” that points to “/wiki/Science”. The ﬁrst thing to know about HTML is that tags are written inside of &lt; and &gt;, and a closing tag is denoted via a &lt;/ and &gt;. Every opening tag must have a closing tag (save for a few self-closing tags), and the content of the tag is

1

contained between the opening and closing tags. Notice above that &lt;html&gt; is closed by &lt;/html&gt; at the bottom of the document, and likewise for &lt;head&gt;, &lt;body&gt;, &lt;p&gt;, and &lt;a&gt;.

Here are some different types of tags:

• &lt;html&gt; tags mark the start and end of the website. There can be only one html section.

• &lt;head&gt; marks the beginning of the metadata section of a website. The head section normally contain tags that

load page materials (fonts, scripts).

• &lt;script&gt; tags contain JavaScript, or reference an external JavaScript ﬁle that should be loaded and ran.

• &lt;body&gt; marks the main content of a website. This is where most visible text will be located.

• &lt;p&gt; tags are used to denote different paragraphs.

• &lt;a&gt; tags represent hyperlinks and will be a main focus of this project. The href parameter contains the des- tination of the link, and the text between the &lt;a&gt; tags contains the visible text. When a link does not cross domains (i.e. the part between the https:// and the .com/.org/…), it can start with a / to move between pages in the same domain easily. For example, when the source page is also on the https://wikipedia.org domain, the link https://wikipedia.org/wiki/Computer can be accessed either by the parameter href=”https://wikipedia.org/wiki/Computer” or the parameter href=”/wiki/Computer” . Since we are starting on Wikipedia and ending on Wikipedia, we only want to follow links in the form href=”/wiki/*”, since that will ensure that we stay on the https://wikipedia.org domain.

Your ﬁrst task is to implement the get_links_in_page function inside of py_wikiracer/wikiracer.py.

This function accepts the HTML code for a single webpage and should ﬁnd all of the valid links inside of that page. For instance, running the function with the example HTML code above should return [“/wiki/Computer”]. Be sure to return links in the order that they appear, and be sure to ﬁlter out duplicate entries.

Additionally, there are a few characters that will invalidate a link in our game. These characters are the colon, pound sign, forward slash, and question mark. You should not hardcode these four characters into being disallowed, but rather use the DISALLOWED global variable provided inside py_wikiracer/wikiracer.py. If a link is found with one of these characters, you can pretend it doesn’t exist in the HTML. Also, if a link is found that doesn’t start with /wiki/, then you can pretend it doesn’t exist, since it could be an external link that takes us outside of Wikipedia.

If you want to play around with HTML, you can save the example script above in an .html ﬁle, and then open it with a web browser. You can (and should) look around real websites’ HTML by using the Inspect Element feature avaliable on most web browsers. In Google Chrome, F12 opens the Inspect Element menu, where you can click on a speciﬁc piece of text within a website to look at the underlying HTML code. Before starting the project, we encourage you to look at different websites’ HTML structure, and see how the links look on Wikipedia. You can also save a ﬁle with Ctrl-S to download its HTML markup and examine it with a text editor.

To accomplish your task, you should make good use of Python’s built in string functions to search for identiﬁable

landmarks near links. You can use Regex to ﬁnd links as well (though be wary of speed).

2.2 Shortest Path Routines

The second part of the assignment is to implement 3 different search routines to create a naive wikiracer. We use Wikipedia as our graph, with websites representing vertices and links representing edges, and we want to efﬁciently tra- verse Wikipedia to ﬁnd the shortest path between two vertices. Each of these three (bfs, dfs, and Dijkstra’s) share a lot of ideas, so be sure to reﬂect this in your code reuse. They should be written inside of py_wikiracer/wikiracer.py in their respective class. Each of these routines accepts as input a start page and goal page. They should use py_wikiracer/internet.py’s Internet class to download a page, and then Parser’s get_links_in_page to ﬁnd a node’s neighbors.

For BFS and DFS, you can think of Wikipedia as a directed, unweighted graph. In BFS, your goal is to spread out evenly across the graph. DFS is similar to BFS, except that you are almost always clicking the last link inside of each webpage, unless you have been there already. This makes it pretty impractical for actually searching for something in Wikipedia, but a useful exercise nonetheless.1

1See https://en.wikipedia.org/wiki/Wikipedia:Getting_to_Philosophy for an interesting result of this behavior.

2

Dijkstra’s algorithm ﬁnds the lowest cost path between two nodes in a weighted graph. In our case, Wikipedia links have no weight, so we pass in a weighting function to Dijkstra’s algorithm that creates a weight given a linked start and end node. For example, if /wiki/Computer has a link to /wiki/Hippopotamus, then the weight of that edge would be costFn(“/wiki/Computer”, “/wiki/Hippopotamus”). By default, costFn simply takes the length of the second argument as the weight, but your code should work with any cost function.

2.3 Wikiracer

• Look for links that are substrings or superstrings of the goal URL (be careful with pages like /wiki/A matching

everything, though).

• Load the “goal” page, and perform a shallow BFS from the goal page. During your search from the source, when you ﬁnd one of these links in a page that is near the goal, prioritize it, hoping that it bidirectionally links back to the goal.

• Use Internet.get_random to ﬁlter out common links between pages, or for other uses.

3 Tips

3.1 Using the Internet

To access the internet, we provide the Internet class. This class contains a internet.get_page method, which returns the HTML associated with a link provided in the form “/wiki/Something”. Internally, the Internet class caches HTML ﬁles to disk so that you can test without having to download Wikipedia pages from the internet every time. Be sure to test your implementation’s runtime from an empty cache, and ensure that it ﬁnishes in a reasonable amount of time. At the moment, the autograder will timeout at around 100 seconds per function call, and it will not have the ﬁles cached.

Each search problem (BFS, DFS, Dijkstra’s, and Wikiracer) is contained within its own class and has its own local

self.internet. Be sure to use this local variable when accessing webpages.

3.2 Karma

3

4 What To Turn In

Use the Canvas website to submit a single ZIP ﬁle that contains your modiﬁed py_wikiracer folder. Speciﬁc instructions can be found at this course’s EdX site under the “Submission Instructions” page. Canvas is available at http://canvas.utexas.edu/.

Important Details.

existing implementations of a Wikiracer, as that is a violation of our course policies.

Acknowledgments. This assignment was created by Calvin Lin and Matthew Giordano. Special thanks to Ali Malik for inspiration.

4
