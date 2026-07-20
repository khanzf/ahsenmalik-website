---
title: "You're Almost There"
description: "Check your email to confirm your calendar reminder subscription."
---

<p id="cal-received-intro">Thank you for signing up! We've sent a confirmation email to <strong id="cal-email-display"></strong>. Please click the link in that email to verify your address — you have <strong>24 hours</strong> before the link expires.</p>

<p>Once confirmed, you'll start receiving election calendar reminders automatically.</p>

<p>If you don't see the email within a few minutes, check your spam or junk folder.</p>

<script>
var params = new URLSearchParams(window.location.search);
document.getElementById('cal-email-display').textContent = params.get('email') || '(unknown)';

if (params.get('scope') === 'address_not_found') {
  var notice = document.createElement('p');
  notice.className = 'cal-notice';
  notice.innerHTML = '<strong>Address Not Found</strong> &mdash; We couldn\'t match your address to a voting district, so until it\'s corrected, you\'ll only get reminders for Virginia statewide elections, not your local ones. Enter your address again on the <a href="/calendar-reminders/">sign-up form</a> to fix this.';
  document.getElementById('cal-received-intro').insertAdjacentElement('afterend', notice);
}
</script>
