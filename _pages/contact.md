---
layout: page
title: İletişim
permalink: /contact
comments: false
---

<form action="https://formspree.io/{{site.email}}" method="POST">    
<p class="mb-4">{{site.name}} ilet iletişim için formu doldurun. Mümkün olan en kısa sürede size döneceğiz!</p>
<div class="form-group row">
<div class="col-md-6">
<input class="form-control" type="text" name="name" placeholder="İsim*" required>
</div>
<div class="col-md-6">
<input class="form-control" type="email" name="_replyto" placeholder="E-Posta Adresi*" required>
</div>
</div>
<textarea rows="8" class="form-control mb-3" name="message" placeholder="Mesajınız*" required></textarea>    
<input class="btn btn-dark" type="submit" value="Gönder">
</form>
