# Facebook activity log (timeline) cleanser
## About
Facebook activity log (timeline) cleanser is a script that can be run in FireFox scratchpad that will automatically delete posts on your activity log.

It is a JavaScript code that does nothing more than simulate the mouse clicks. You can set it running and leave it to complete. It takes appx. 10 seconds per post (so may take a while if you have been busy).

It is not perfect and a bit hacky, but at the time of coding I could not find any other working tools. 

## Usage
* Open FireFox
* Login to facebook
* Browse to your activity log
* Scroll to the year you want to delete, then keep scrolling so all the posts that you want deleted are visible
* Scroll back up to the first post of that year
* Open the scratchpad (Menu > Developer > Scratchpad)
* Paste in the JavaScript code from below
* Edit var year = 20xx to whatever you require
* Click run and watch as your posts are deleted

## Notes
### Will be deleted:
* Your posts/statuses
* Posts you have made on other peoples timelines
* Comments you've made
* Likes on groups/comments/images

### Will not be deleted:
* Photos your tagged in
* Posts people have made on your timeline
* Friends
* Anyting else not listed above...

## The Code
    
    var year = 2007;

    var buttons = document.querySelectorAll('#year_'+year+' [aria-label="Allowed on timeline"]'), i;
    for (i = 0; i < buttons.length; ++i) {
        timeout = i*10000
        doSetTimeout(i, timeout);
    }

    function doSetTimeout(i, timeout) {
      setTimeout(function() { 
          buttons[i].scrollIntoView( true );
          window.scrollBy(0, -200);
          buttons[i].focus();
      }, timeout);
      setTimeout(function() { 
          buttons[i].click();
      }, (timeout+1000));
      setTimeout(function() { 
          owns = buttons[i].getAttribute('aria-owns');
          link = document.querySelectorAll('#'+owns+' [ajaxify^="/ajax/timeline/delete/confirm"]'), i;
          link2 = document.querySelectorAll('#'+owns+' [ajaxify^="/ajax/timeline/all_activity/remove_content.php"]'), i;
      
          if(link[0]){
             link[0].click();
          }
          if(link2[0]){
             link2[0].click();
          }

      }, (timeout+2000));
      setTimeout(function() { 
            if(link[0]){
                form = document.querySelectorAll('[action^="/ajax/timeline/delete"] button'), i;
                form[0].click();
            }

       }, (timeout+4000));
    
    } 
    
# Testing
* I have tested the module on 29th August 2015 and it worked for me
* There is no way to recover posts so make sure you backup your account first!
* Use at your own risk, I do not offer any guarantee or warranty

# Licence
The code is released under [GNU GPL v3](http://www.gnu.org/licenses/gpl-3.0.en.html)