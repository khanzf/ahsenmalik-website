---
title: "Calendar Reminders"
description: "Sign up for automatic calendar alerts sent via email so you never miss an election."
---

<p class="cal-intro">Sign up below and we'll send you automatic calendar alerts via email before every election — so you're always ready to vote.</p>

<form class="cal-form" action="https://ycjk1fzle1.execute-api.us-east-2.amazonaws.com/register" method="post">

  <div class="cal-field cal-field--required">
    <label for="cal-first-name">First Name <span aria-hidden="true">*</span></label>
    <input type="text" id="cal-first-name" name="first_name" required autocomplete="given-name">
  </div>

  <div class="cal-field">
    <label for="cal-middle-name">Middle Name <span class="cal-optional">(optional)</span></label>
    <input type="text" id="cal-middle-name" name="middle_name" autocomplete="additional-name">
  </div>

  <div class="cal-field cal-field--required">
    <label for="cal-last-name">Last Name <span aria-hidden="true">*</span></label>
    <input type="text" id="cal-last-name" name="last_name" required autocomplete="family-name">
  </div>

  <div class="cal-field cal-field--required">
    <label for="cal-email">Email Address <span aria-hidden="true">*</span></label>
    <input type="email" id="cal-email" name="email" required autocomplete="email">
  </div>

  <div class="cal-field">
    <label for="cal-phone">Phone Number <span class="cal-optional">(optional)</span></label>
    <input type="tel" id="cal-phone" name="phone" autocomplete="tel">
  </div>

  <p class="cal-section-note">Your address is used to verify your voting district and ensure you receive alerts relevant to your elections.</p>

  <div class="cal-field cal-field--required">
    <label for="cal-address1">Street Address <span aria-hidden="true">*</span></label>
    <input type="text" id="cal-address1" name="address_line1" required autocomplete="address-line1" placeholder="742 Evergreen Terrace">
  </div>

  <div class="cal-field">
    <label for="cal-address2">Address Line 2 <span class="cal-optional">(optional)</span></label>
    <input type="text" id="cal-address2" name="address_line2" autocomplete="address-line2" placeholder="Apt 300">
  </div>

  <div class="cal-field cal-field--required">
    <label for="cal-city">City <span aria-hidden="true">*</span></label>
    <input type="text" id="cal-city" name="city" required autocomplete="address-level2" placeholder="Springfield">
  </div>

  <div class="cal-field">
    <label for="cal-state">State</label>
    <input type="text" id="cal-state" name="state" value="Virginia" readonly class="cal-readonly">
  </div>

  <div class="cal-field cal-field--required">
    <label for="cal-zip">Zip Code <span aria-hidden="true">*</span></label>
    <input type="text" id="cal-zip" name="zip" required maxlength="5" inputmode="numeric" pattern="[0-9]{5}" autocomplete="postal-code">
  </div>

  <p class="cal-required-note"><span aria-hidden="true">*</span> Required fields</p>

  <div class="cf-turnstile cal-turnstile" data-sitekey="0x4AAAAAADrw9spfC-hD5DD_"></div>

  <button type="submit" class="btn btn-primary cal-submit">Sign Me Up</button>

</form>

<script>
(function () {
  var zip = document.getElementById('cal-zip');
  if (zip) {
    zip.addEventListener('input', function () {
      this.value = this.value.replace(/\D/g, '').slice(0, 5);
    });
  }

  var phone = document.getElementById('cal-phone');
  if (phone) {
    phone.addEventListener('input', function () {
      var digits = this.value.replace(/\D/g, '').slice(0, 10);
      var fmt = '';
      if (digits.length > 6) {
        fmt = '(' + digits.slice(0,3) + ') ' + digits.slice(3,6) + '-' + digits.slice(6);
      } else if (digits.length > 3) {
        fmt = '(' + digits.slice(0,3) + ') ' + digits.slice(3);
      } else if (digits.length > 0) {
        fmt = '(' + digits;
      }
      this.value = fmt;
    });
  }
})();
</script>
