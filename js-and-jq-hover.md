---
title: "jQuery hover migration to Javascript"
date: 2025-03-30T02:14:39+08:00
---

- jq
```
    $(document).ready(function () {
        $(".thumbs a").hover(function () {
            var largePath = $(this).attr("imgLnk");
            var largeAlt = $(this).attr("title");
            var productLnk = "/product/" + $(this).attr("path");

            $("#largeImg").attr({ src: largePath, alt: largeAlt });

            $("#largeLnk").attr({ href: productLnk });

            return false;
        });

    });
```
- js
```
    function setLargeImage(id, path, title) {
        const element = document.querySelector(`#${id}`);
        element.setAttribute("src", path);
        element.setAttribute("alt", title);
    }
    
    function setLargeImageLink(id, link) {
        const element = document.querySelector(`#${id}`);
        element.setAttribute("href", link);
    }
    
    const thumbImages = document.querySelectorAll(".thumbs a");

    thumbImages.forEach( (item, index) => {
        item.addEventListener("mouseover", () => {
            const parentElement = event.target.parentNode;
            console.log(parentElement);

            let largePath = parentElement.getAttribute("imgLnk");
            let largeAlt = parentElement.getAttribute("title");
            let productLnk = "/product/" + parentElement.getAttribute("path");

            setLargeImage(CSS.escape("largeImg"), largePath, largeAlt);
            setLargeImageLink(CSS.escape("largeLnk"), productLnk);
        })
    });
```

## Ref
- [image - JQuery hover](https://bootstrapfriendly.com/blog/how-to-show-another-big-image-on-hover-using-jquery)
- [get attribute](https://www.w3schools.com/jsref/met_element_getattribute.asp)
- [set attribute](https://www.w3schools.com/jsref/met_element_setattribute.asp)
- [mouseover - event](https://developer.mozilla.org/en-US/docs/Web/API/Element/mouseover_event)
- [querySelectorAll](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)
- [parentNode](https://stackoverflow.com/a/29168819)
