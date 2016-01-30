---
layout: post
title: Rails, jQuery AJAX, respond_to, Google Chrome
created: 1264287600
comments: true
---
<code class="javascript">
 jQuery.ajaxSetup({
   'beforeSend': function(xhr) {
     // First unset it, then set (which otherwise appends)
     xhr.setRequestHeader("Accept", "");
     xhr.setRequestHeader("Accept", "text/javascript");
     }
 });
</code>

Ez valamiért nincs hatással a fent nevezett böngészőkre. Hoszas keresgélés és egy zuhany alatt kipatant ötlet eredményeként az application_controller-ben megszületett az alábbi néhány kódsor:

 
<code class="ruby">
  def correct_safari_and_ie_accept_headers
    request.accepts.sort!{ |x, y| y.to_s == 'text/javascript' ? 1 : -1 } if request.xhr?
  end
</code>

Valószínűleg nem csak én használok jQuery-t Rails-el. Remélem, másnak is hasznos lesz ez az igazán rövid és hatásos megoldás. Apró megjegyzés, bár szerintem magától értetődő, hogy a fenti funkciót egy before_filter segítségével meg kell hívni
