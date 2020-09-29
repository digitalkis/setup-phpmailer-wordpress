# This is an example of establishing a SMTP connection using the `phpmailer_init` action without plugins.

## Use your wp-config.php to set the values for host, username, password...

```php
<?php
...
define( 'SMTP_USER', 'email@example.com' ); // Username to use for SMTP authentication
define( 'SMTP_PASS', 'your_password' ); // Password to use for SMTP authentication
define( 'SMTP_HOST', 'smtp.example.com' ); // The hostname of the mail server
define( 'SMTP_FROM', 'email@example.com' ); // SMTP From email address
define( 'SMTP_FROM_NAME', 'My WordPress Site' );    // SMTP From name
define( 'SMTP_PORT', '465' ); // SMTP port number - likely to be 25, 465 or 587
define( 'SMTP_SECURE', 'ssl' ); // Encryption system to use - ssl or tls
define( 'SMTP_AUTH', true); // Use SMTP authentication (true|false)
define( 'SMTP_DEBUG', 0 ); //Options are 0,1,2,3,4 - More details here: https://github.com/PHPMailer/PHPMailer/wiki/SMTP-Debugging

```
## Edit your functions.php and add the following code:

```php
<?php
...
add_action( 'phpmailer_init', 'configure_smtp_authentication' );
function configure_smtp_authentication( $phpmailer ) {
    
  $phpmailer->isSMTP();     
  $phpmailer->Host = SMTP_HOST;
  $phpmailer->SMTPAuth = SMTP_AUTH; // Force it to use Username and Password to authentication
  $phpmailer->Port = SMTP_PORT;
  $phpmailer->Username = SMTP_USER;
  $phpmailer->Password = SMTP_PASS;
  $phpmailer->SMTPSecure = SMTP_SECURE;
  $phpmailer->From = SMTP_FROM;
  $phpmailer->FromName = SMTP_FROM_NAME;
  $phpmailer->SMTPDebug = SMTP_DEBUG;
    
}
```

## Bonus:
If you need to log errors:

```php
<?php
...
add_action('wp_mail_failed', 'log_smtp_errors', 10, 1);
function log_smtp_errors( $wp_error ) {
  $fn = ABSPATH . '/email.log'; // say you've got a email.log file in your server root
  $fp = fopen($fn, 'a');
  fputs($fp, "Mailer Error: " . $wp_error->get_error_message() ."\n");
  fclose($fp);
}
```
