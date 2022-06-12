# Introduction to Accessible Rich Internet Applications attributes

## About the Web Accessibility Initiative

The [Web Accessibility Initiative's Accessible Rich Internet Applications](https://www.w3.org/TR/wai-aria/#intro_ria_accessibility) (WAI-ARIA or just ARIA) offers  attributes that are applied to HTML elements, making web content and applications more accessible to people with disabilities. The core ARIA component `role`, allows screen readers to more explicitly describe the intentions of web content or apps to people; it defines what an element does.

***Note***
Elements can only have one `role`.

There are 6 categories for [WAI-ARIA roles](https://www.w3.org/TR/wai-aria-1.1/#roles_categorization):

* **Landmark** roles typically denote large areas of a document. Some examples are `banner`, `search`, and `region`
* **Widget** roles help in making interactive elements accessible. `button`, `checkbox`, and `radio` are common examples of widget roles
* **Window** roles help in creating a sub-window within your webpage. The two roles that comprise window roles are `alertdialog` and `dialog`
* **Document structure** roles give information about a non-interactive, static section of the page. Common document structure roles are `article`, `table`, and `heading`
* **Live region** roles help you inform assistive technologies about dynamic content of a webpage. They work together with the `aria-live` attribute. `alert`, `log`, and `timer` are commonly used live region roles
* **Abstract** roles are the foundation on which other WAI-ARIA roles are based on. Abstract roles are used by browsers and not developers

ARIA also provides attributes, which are slightly different from roles. ARIA attributes are prefixed by `aria-` and added to HTML in the same way as `role`, with a wide range of additional attributes available. There are two types of attributes, [states and properties](https://www.w3.org/TR/wai-aria/#states_and_properties):

* The value of states are bound to change as a result of user interaction
* The value of properties is less likely to change

## Why ARIA matters

When a developer intends to implement ARIA, using either [roles](https://www.w3.org/TR/wai-aria-1.0/roles) and/or [states and properties](https://www.w3.org/TR/wai-aria-1.0/states_and_properties), they must be used per ARIA guidelines. When values are invalid, the attributes could render web content useless; having no effect on a taken action or reporting inaccurate UI information.

## Common ARIA mistakes

While ARIA is important in its ability to enhance the web browsing experience for AT, there are problems that can arise from a development standpoint. Common mistakes include:

* **Incorrect WAI-ARIA Syntax**: includes issues like case sensitivity, incorrect spelling, invalid role declarations or the role does not exist
* **Invalid ID value references**: attributes will only accept [supported states and properties](https://www.w3.org/TR/wai-aria-1.1/#state_prop_def)
* **Allowed or required WAI-ARIA attributes missing**: WAI-ARIA provides specific guidance on how attributes are to be [implemented in host languages](https://www.w3.org/TR/2017/REC-wai-aria-1.1-20171214/#host_languages), like semantic HTML or JavaScript

## Troubleshooting resources

WAI-ARIA explicitly defines attributes, their values, and how they can be used. In addition to the links mentioned throughout this page, the following documentation can also be used to troubleshoot common ARIA issues:

* [Customizable Quick Reference](https://www.w3.org/WAI/WCAG21/quickref/?showtechniques=145#top) is a comprehensive and highly customizable view of ARIA guidelines and techniques for applying ARIA attributes
* [Using ARIA](https://w3c.github.io/using-aria/) contains content on how to add accessibility information to elements
* [WAI-ARIA Authoring Practices 1.1](https://www.w3.org/TR/wai-aria-practices/) provides guidance on successfully applying WAI-ARIA attributes
