---
title: "There Was a Problem"
description: "There was a problem processing your request."
---

<div id="error-box" class="error-box">
  <p id="error-message">Loading...</p>
  <p id="error-requestid" class="error-requestid"></p>
</div>

<script>
(function () {
  var params = new URLSearchParams(window.location.search);
  var code = params.get('code');
  var requestId = params.get('requestid') || '(none)';

  var messages = {
    token_missing: "Your request did not contain the request JWT token. Please go back to the request page, hit reload, and re-submit the form.",
    token_invalid: "Your request did not contain a valid JWT token. Please go back to the request page, hit reload, and re-submit the form.",
    token_expired: "Your request token is expired. You might have waited too long to submit your form. Please go back to the request page, hit reload, and re-submit the form.",
    token_missing_fields: "Your JWT token was missing required fields. Please go back to the request page, hit reload, and re-submit the form. If the issue persists, this is not an issue on your end, this is a problem with NoVA Turnout.",
    database_error: "There was an internal database error. Please try your request again. If the issue persists, this is not a problem on your end, this is a problem with NoVA Turnout.",
    cloudflare_rejected: "There was an error with the Cloudflare anti-spam protection. Please try again after a few minutes, on a different browser or different computer.",
    email_send_failed: "Unable to send an email to your address. Either your email is mistaken or there was an internal error.",
    rate_limit: "An email was already sent to your address within the last 5 minutes. Please wait for that window to pass before trying again."
  };

  var message = messages[code] || "Something went wrong with your request. Please go back to the request page, hit reload, and re-submit the form. If the issue persists, this is not a problem on your end, this is a problem with NoVA Turnout.";

  document.getElementById('error-message').textContent = message;
  document.getElementById('error-requestid').textContent = 'Request ID: ' + requestId;
})();
</script>
