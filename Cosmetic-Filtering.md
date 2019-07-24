

cosmetic filtering API call:

```
        chrome.tabs.insertCSS({
          code: `${rule} {display: none !important;}`,
          cssOrigin: 'user',
          runAt: 'document_start'
        })
```


`display: none !important` fixes https://github.com/brave/brave-browser/issues/3041

`cssOrigin: 'user'` fixes https://github.com/brave/brave-browser/issues/4348 as follows: 


>By default, rules in an author’s style sheet override those in a user’s style sheet, which override those in the user-agent’s default style sheet. To balance this, a declaration can be made important, which increases its weight in the cascade and inverts the order of precedence.

so it’d be `author > user > user-agent`  
`!important` sets it to  
`user-agent > user > author`

the scenarios are as follows:  
page doesn’t have !important: user stylesheet has !important (it looks like this overrides any elem that doesn’t have !important)  
page does have !important: user stylesheet !important > author !important  

spec: https://www.w3.org/TR/css3-cascade/#cascading-origins

