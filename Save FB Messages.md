## How to download old facebook messages:

1. Go to [m.facebook.com/messages](m.facebook.com/messages) and open the conversation you want to save
2. Open your javascript console with `Opt` + `Cmd` + `j` / `Ctrl` + `Shift` + `j` (chrome)
3. Enter:
```
setInterval(function () {
document.getElementById('see_older')
.getElementsByClassName('content')[0].click();
}, 500);
```
4. Wait
5. Print them to a .pdf (remove headers and footers to make it look nicer)
