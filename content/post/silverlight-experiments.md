+++
date = "2010-03-24T00:42:44-07:00"

title = "Silverlight Experiments"
tags = ["c#", "silverlight"]
categories = ["Software Development"]
+++

I haven't stopped working on XNA game development, but I have had to take some time recently to further hone my <a href="http://www.silverlight.net">Silverlight </a>skills. Silverlight 4 RC was recently released and the full version should be out in a month or so, so I decided that I better start playing with it now (in reality, I should have been playing with it since the beta...).

One of the new controls that I was most excited about is the WebBrowser control. You would think that already being in a web browser, having the ability to render HTML wouldn't be that important within Silverlight, but it is. The good news is, this will help make that much easier. The bad news is, it only works in Out of Browser mode. I assume this is per the usual security concerns that really hinder development on the web IMHO. But, once it becomes a trusted application that the user has chosen to install, then you can use it. <a href="http://superiorcode.com/blog/wp-content/uploads/2010/03/sl4_browser.png"><img class="alignright size-medium wp-image-126" title="sl4_browser" src="http://superiorcode.com/blog/wp-content/uploads/2010/03/sl4_browser-300x116.png" alt="" width="300" height="116" /></a>

The following properties are the most important ones when working with the WebBrowser control:
<ul>
    <li>Source: gets or sets the URI that should be rendered in the WebBrowser control</li>
    <li>Navigate: specifies the URI that should be loaded in the control (works identical to the Source property)</li>
    <li>NavigateToString: you can also display an on-the-fly generated string of HTML. This can be done using this method.</li>
</ul>
Another great new control is the RichTextBox.

This new control includes clipboard support, text formatting and BiDi (Bidirectional) text support for input and output. The control also enables applications to show rich text and allows users to input formatted text (Bold, Italic, Underline, Font family, Font size and Font color), highlighting of text, and embedded elements such as Hyperlinks and Images.

Another useful feature is Silverlight's Drop Target API. This feature allows end users to drag &amp; drop word documents directly in to the RichTextBox control. The control also can return Xaml allowing the format of rich text to easily be stored and retrieved.

I have created a section on this site for <a href="http://superiorcode.com/blog/?page_id=105">Silverlight Demos</a> that I will be posting various experiments to. At the moment, there is only one, and it is Silverlight 3, but soon I will post more.
