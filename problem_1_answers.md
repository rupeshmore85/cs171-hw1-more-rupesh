Problem 1:
 
1. What does the colspan="3" attribute of the <th> node do?
 
Ans: The colspan attribute specifies the number of columns a cell should span. colspan="3" in the <th> element specifies that the header should be spanned with 3 columns (i.e. Rank, State and Rate columns will be merged to form a single cell.
 
2. List all the styles (e.g. border width, text alignment, etc.) applied to the th element containing "Rank". For each, state whether they are set as an HTML attribute or a CSS style and describe them in a few words. Include only styles directly applied to the element, not styles inherited/cascading from parent elements or styles from the default user agent stylesheet. Exclude overwritten styles. For HTML attributes, state the CSS equivalent.
 
Ans:
   i. align='center'. This html attribute is used to align the test i.e. Rank in the center.
   The CSS equivalent is:  {text-align: center;}
   ii. padding: 3px; This is a CSS attribute which sets the padding to 3px. So the test Rank is padded by 3px within the associated rectangle.
   iii. line-height: 1.22em; This is a CSS property which specifies the minimal height of line boxes within the element. Here the line height is set to the length value of 1.22em
   iv. Margin: 0px; The margin property sets or returns the margins of an element. In this case the margin is set to 0.
  
3. What differences do you notice between the DOM inspector and the HTML source? Why would you use the DOM inspector? Why is the HTML source useful?
 
Ans: 
i.a. The HTML source is the raw HTML that is unadulterated by any client-side scripts. It is the direct response of the HTTP request to the server.
The DOM, on the other hand, is the same HTML structure that has been modified by JavaScript.

b. The Source Code reads the page’s HTML as if you opened it in a text editor. The source code reflects your HTML structure before any JavaScript is loaded. While the contents can’t be edited, it’s useful to see the HTML Safari receives from the server.
The DOM tree is fully editable. The edits are temporary, so any changes that are made are lost in a browser refresh. To save changes you make to the DOM tree, select the root html node and press Command-C, which copies the DOM structure to you clipboard as HTML.

ii. Why would you use the DOM inspector?
DOM Inspector is a tool that can be used to inspect and edit the live DOM of any web document or XUL application. DOM inspector like Firebug helps to
There are two kinds of objects and functions - those that are part of the standard DOM, and those that are from your own JavaScript code. 
Firebug can tell the difference, and shows you your own script-created objects and functions in bold at the top of the list. It can find DOM objects quickly and then edit them on the fly.

iii. HTML source tells you the structure of the HTML page. It gives the whole details of all the HTML,CSS elements used in the code.
Searching an element in the HTML source is simple than to find that same element in the DOM. 
