# JavaScript Codes for Web Scraping

This document provides a collection of JavaScript snippets written to assist in developing web scraping software. These scripts focus on tasks like interacting with webpage elements, manipulating the DOM, and formatting/simplifying complex web structures. 

These JavaScript codes were initially crafted to complement the functionality of [WebHarvy](https://www.webharvy.com), a powerful and user-friendly [web scraping software](https://www.webharvy.com). However, their utility extends beyond WebHarvy, making them equally effective for other web scraping tools and frameworks. Developers using custom scripts with libraries like Puppeteer, Playwright, or similar automation frameworks will also find these snippets useful.

## Smooth scroll a list of items - simulate slow list scrolling

```javascript
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function scrollList() {

	list = document.getElementsByClassName('data-cards')[0];
	for (var i = 0; i < list.childElementCount; i++) {
		list.children[i].scrollIntoView();
		await sleep(100);
	}
}

scrollList();
```

## Delete an element

```javascript
element.parentElement.removeChild(element);
```

### Delete all elements of a given class name

```javascript
var theaders = document.getElementsByClassName('js-tournament');
for (var i=theaders.length-1; i >=0; i--) {
	theaders[i].parentElement.removeChild(theaders[i]);
}
```

## Click all elements on page with a given class name

```javascript
var elements = document.getElementsByClassName("expland-item");
for(i=elements.length-1; i >= 0; i--) {
	elements[i].click();
}
```

## Get page URL

```javascript
window.location.href
```

## Submit form

```javascript
formEl.submit();
```

## Navigate back

```javascript
window.history.back();
```

## Input Text

### Normal method

```javascript
const changeValue = (element, value) => {
  const event = new Event('input', { bubbles: true });
  event.simulated = true;
  element.value = value;
  element.dispatchEvent(event);
}
```

### For frameworks like React/Vue etc.

```javascript
const input = document.querySelector(“selector-string”); 
Object.defineProperty(input, 'value', { value: 'paste-text-here’', writable: true}); 
input.dispatchEvent(new Event('input', {{ bubbles: true }}));
```

## Scroll to end to activate infinite-scrolling

```javascript
var list = document.querySelector('selectorstring').children;
list[list.length-1].scrollIntoView();
```

## Merge multiple tables in to a single table

```javascript
var tables = document.querySelectorAll("tbody");
for (var i = 1; i < tables.length; i++) {
    var rows = tables[i].querySelectorAll("tr");
    for (var j = 0; j < rows.length; j++) {
        tables[0].appendChild(rows[j]);
    }
}
```

## Merge various groups/list in to the first group/list

```javascript
var groups = document.getElementsByClassName('groups-class-name');
var parent = groups[0];
for (var i = groups.length - 1; i >= 1; i--) {
    var group = groups[i];
    for (var j = group.children.length - 1; j >= 0; j--) {
   	 parent.appendChild(group.children[j]);
    }
}
```

### Merge by preserving order

The above code messes up the order of items. If you want the items to appear in the same order even after merging, use the code given below.

```javascript
var groups = document.getElementsByClassName('groups-class-name');
var anchor = null;
var parent = groups[0];
for (var i = groups.length - 1; i >= 1; i--) {
    var group = groups[i];
    for (var j = group.children.length - 1; j >= 0; j--) {
   	 	anchor = parent.insertBefore(group.children[j], anchor);
   	}  	 
}
```

## Select data from #shadow-root element

The content (HTML) from within the #shadow-root element should be brought outside, so that it can be selected or scraping. 

```javascript
container = document.getElementById('shadow-root-parent-id');
document.body.innerHTML = container.shadowRoot.querySelector('div').innerHTML;
```

**containter** in the above code is the parent element of the shadow-root element. The design of the page will be lost when the above code is run, but since HTML structure is preserved (for the data content which we are interested in) it can be selected and scraped easily. 

### Alternative Solution

```javascript
function querySelectorAllShadows(selector, el = document.body) {
  const childShadows = Array.from(el.querySelectorAll('*')).
    map(el => el.shadowRoot).filter(Boolean);
  const childResults = childShadows.map(child => querySelectorAllShadows(selector, child));
  const result = Array.from(el.querySelectorAll(selector));
  return result.concat(childResults).flat();
}
document.body.innerHTML = querySelectorAllShadows('#app')[1].innerHTML;
```

querySelectorAllShadows('#app') returns all elements which confirm to the selector #app in the page (both inside and outside the shadow-dom). [1] is the desired one inside the shadow-root element, this we will have to find via inspection. In this case the #app selector is the selector of the container inside the shadow-root, found using Chrome developer tools. 

## Hover mouse over an element to display a popup/tooltip using jQuery

```javascript
$(".username.mo").mouseover();
```

The selecter selects all elements with the given class name and hovers over them.

### Vanilla JavaScript

```javascript
var event = new Event('mouseover'); 
element.dispatchEvent(event);
```

## To store data on an element as a new attribute

```javascript
element.setAttribute('attr-name', 'attr-data');
```

The saved can be read using the *getAttribute* method. 

## Select dropdown option

```javascript
element = document.getElementById('dropdown-id'); 
element.selectedIndex = index-to-select; // example: 5
event = new Event('change'); element.dispatchEvent(event);
```


## Disable all mouseover, hover events on page

```javascript
$('div').unbind("mouseenter mouseleave hover");
```

## Load jQuery on page

```javascript
var script = document.createElement('script');
script.type = "text/javascript";
script.src = "https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js";
document.getElementsByTagName('head')[0].appendChild(script);
```

## Store variables which can be accessed across different pages

window.name can store variables which will be retained even in the following links and new pages.

```javascript
window.name=location.href; 
```

Above code stores current page URL in **window.name**. From a new page, we can navigate back to the starting page using:

```javascript
location.href=window.name;
```

The above code is useful to navigate back to a listings page after opening a popup, when the poppup offers no way to close/navigate-back. 
