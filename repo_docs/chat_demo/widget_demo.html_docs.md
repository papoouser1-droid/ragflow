# File Documentation: chat_demo/widget_demo.html

## File Metadata

- **Path**: `chat_demo/widget_demo.html`
- **Extension**: `.html`
- **Lines**: 154
- **Characters**: 5,541
- **Size**: 5,555 bytes
- **Purpose**: General file in the repository

## Original Source

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Floating Chat Widget Demo</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 40px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: white;
        }
        
        .demo-content {
            max-width: 800px;
            margin: 0 auto;
        }
        
        .demo-content h1 {
            text-align: center;
            font-size: 2.5rem;
            margin-bottom: 2rem;
        }
        
        .demo-content p {
            font-size: 1.2rem;
            line-height: 1.6;
            margin-bottom: 1.5rem;
        }
        
        .feature-list {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 2rem;
            margin: 2rem 0;
        }
        
        .feature-list h3 {
            margin-top: 0;
            font-size: 1.5rem;
        }
        
        .feature-list ul {
            list-style-type: none;
            padding: 0;
        }
        
        .feature-list li {
            padding: 0.5rem 0;
            padding-left: 1.5rem;
            position: relative;
        }
        
        .feature-list li:before {
            content: "✓";
            position: absolute;
            left: 0;
            color: #4ade80;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="demo-content">
        <h1>🚀 Floating Chat Widget Demo</h1>
        
        <p>
            Welcome to our demo page! This page simulates a real website with content. 
            Look for the floating chat button in the bottom-right corner - just like Intercom!
        </p>
        
        <div class="feature-list">
            <h3>🎯 Widget Features</h3>
            <ul>
                <li>Floating button that stays visible while scrolling</li>
                <li>Click to open/close the chat window</li>
                <li>Minimize button to collapse the chat</li>
                <li>Professional Intercom-style design</li>
                <li>Unread message indicator (red badge)</li>
                <li>Transparent background integration</li>
                <li>Responsive design for all screen sizes</li>
            </ul>
        </div>
        
        <p>
            The chat widget is completely separate from your website's content and won't 
            interfere with your existing layout or functionality. It's designed to be 
            lightweight and performant.
        </p>
        
        <p>
            Try scrolling this page - notice how the chat button stays in position. 
            Click it to start a conversation with our AI assistant!
        </p>
        
        <div class="feature-list">
            <h3>🔧 Implementation</h3>
            <ul>
                <li>Simple iframe embed - just copy and paste</li>
                <li>No JavaScript dependencies required</li>
                <li>Works on any website or platform</li>
                <li>Customizable appearance and behavior</li>
                <li>Secure and privacy-focused</li>
            </ul>
        </div>
        
        <p>
            This is just placeholder content to demonstrate how the widget integrates 
            seamlessly with your existing website content. The widget floats above 
            everything else without disrupting your user experience.
        </p>
        
        <p style="margin-top: 4rem; text-align: center; font-style: italic;">
            🎉 Ready to add this to your website? Get your embed code from the admin panel!
        </p>
    </div>

    <iframe id="main-widget" src="http://localhost:9222/next-chats/widget?shared_id=9dcfc68696c611f0bb789b9b8b765d12&from=chat&auth=U4MDU3NzkwOTZjNzExZjBiYjc4OWI5Yj&visible_avatar=1&locale=zh&mode=master&streaming=false"
    style="position:fixed;bottom:0;right:0;width:100px;height:100px;border:none;background:transparent;z-index:9999;opacity:0;transition:opacity 0.2s ease"
    frameborder="0" allow="microphone;camera"></iframe>
  <script>
  window.addEventListener('message',e=>{
    if(e.origin!=='http://localhost:9222')return;
    if(e.data.type==='WIDGET_READY'){
      // Show the main widget when React is ready
      const mainWidget = document.getElementById('main-widget');
      if(mainWidget) mainWidget.style.opacity = '1';
    }else if(e.data.type==='CREATE_CHAT_WINDOW'){
      if(document.getElementById('chat-win'))return;
      const i=document.createElement('iframe');
      i.id='chat-win';i.src=e.data.src;
      i.style.cssText='position:fixed;bottom:104px;right:24px;width:380px;height:500px;border:none;background:transparent;z-index:9998;display:none;opacity:0;transition:opacity 0.2s ease';
      i.frameBorder='0';i.allow='microphone;camera';
      document.body.appendChild(i);
    }else if(e.data.type==='TOGGLE_CHAT'){
      const w=document.getElementById('chat-win');
      if(w){
        if(e.data.isOpen){
          w.style.display='block';
          // Wait for the iframe content to be ready before showing
          setTimeout(() => w.style.opacity='1', 100);
        }else{
          w.style.opacity='0';
          setTimeout(() => w.style.display='none', 200);
        }
      }
    }else if(e.data.type==='SCROLL_PASSTHROUGH')window.scrollBy(0,e.data.deltaY);
  });
  </script>
</body>
</html>
```

## High-Level Overview

This file is part of the RAGFlow repository located at `chat_demo/widget_demo.html`.

Based on the file structure and naming, it appears to be a general file in the repository.

The file contains approximately 154 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough

This file's structure is not automatically analyzed. See the 'Original Source' section for content.

## Code Structure Analysis

- Total lines: 154
- Blank lines: 16 (10.4%)
- Comment lines: ~2 (1.3%)
- Code lines: ~136


## Dependencies and Imports

No explicit dependencies detected or not applicable for this file type.

## Design & Architecture

This file is located in the `chat_demo` directory, specifically within `chat_demo`.

This file contributes to the overall functionality of the RAGFlow system.

## Performance & Complexity

- Contains 4 loop(s) - consider algorithmic complexity

## Security & Safety Considerations

- **Authentication**: Ensure secure password handling and authentication
- **File Operations**: Validate file paths to prevent directory traversal

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `chat_demo/` directory
- Potential test file: `test_widget_demo.html`

## Keywords

Arial, CREATE_CHAT_WINDOW, Chat, Click, Customizable, DOCTYPE, Demo, Features, Floating, Get, Implementation, Intercom, JavaScript, Look, Minimize, Professional, React, Ready, Responsive, SCROLL_PASSTHROUGH, Secure, Show, Simple, TOGGLE_CHAT, The, This, Transparent, Try, U4MDU3NzkwOTZjNzExZjBiYjc4OWI5Yj, UTF, Unread, WIDGET_READY, Wait, Welcome, Widget, Works, i, mainWidget, w

---
*Generated by RAGFlow Repository Documentation Generator*
