# PHP Hotel Management 

Lightweight hotel booking system built with PHP, MySQL and minimal front-end. Includes an admin panel, Paytm payment integration, optional SendGrid email support, and PDF receipt generation via mPDF.

## Repository layout (key files/dirs)

- [admin](admin#L1) — admin panel, admin scripts, `inc` with `db_config.php` and `essentials.php`.
- [ajax](ajax#L1) — AJAX endpoints (login/register, profile, bookings, reviews).
- [inc](inc#L1) — site includes: `header.php`, `footer.php`, `links.php`, `paytm/`, `sendgrid/` helper files.
- [images](images#L1) — image assets (about, carousel, rooms, etc.).
- [css/common.css](css/common.css#L1) — main stylesheet.
- [DATABASE_FILE/hbwebsite.sql](DATABASE_FILE/hbwebsite.sql#L1) — database dump to import.

## Requirements

- PHP 7.4+ (or compatible), with extensions: `mysqli`, `curl`, `openssl`, `gd` (for image functions), `mbstring`.
- MySQL / MariaDB.
- Apache (XAMPP/WAMP) or other web server. Place project inside your web root or configure virtual host.
- Composer is optional — vendor libraries (mPDF, sendgrid helpers) are already included under `admin/inc/mpdf/vendor` and `inc/sendgrid/vendor`.

## Quick setup

1. Copy repository into your web server document root (e.g., `htdocs/hbwebsite`), or update `SITE_URL` in [admin/inc/essentials.php](admin/inc/essentials.php#L1).
2. Import the database: open phpMyAdmin (or mysql CLI) and import `DATABASE_FILE/hbwebsite.sql`.
3. Configure DB connection: edit [admin/inc/db_config.php](admin/inc/db_config.php#L1) and set host, DB name, user and password.
4. Configure basic constants: open [admin/inc/essentials.php](admin/inc/essentials.php#L1) and set:
   - `SITE_URL` to your local URL (e.g., `http://127.0.0.1/hbwebsite/`).
   - `UPLOAD_IMAGE_PATH` if you change document root.
   - Optional SendGrid constants: `SENDGRID_API_KEY`, `SENDGRID_EMAIL`, `SENDGRID_NAME` (only if enabling emails).
5. Paytm settings: edit [inc/paytm/config_paytm.php](inc/paytm/config_paytm.php#L1) and set the merchant keys, MID, WEBSITE, INDUSTRY_TYPE_ID and CALLBACK_URL. `pay_now.php`, `pay_response.php` handle the flow.

## Enable email verification / SendGrid (optional)

- Email verification is disabled by default. To enable transactional emails using SendGrid:
  1. Add your API key and sender details in `admin/inc/essentials.php`.
  2. Follow comments in [ajax/login_register.php](ajax/login_register.php#L1) (lines around register handling) — the code includes in-file comments showing where to enable SendGrid usage.
  3. Adjust the register success alert text in `inc/footer.php` register callback (see `inc/footer.php` around register handling) to match messaging.
  4. Test using a registration flow; check network tab and server logs for errors.

Note: sendgrid helper files are under [inc/sendgrid](inc/sendgrid#L1) and include sample code and examples.

## Admin panel

- Admin UI is in `admin/index.php` with related pages in `admin/` and `admin/ajax/`.
- There is no hardcoded default admin password in the repo — create an admin record in the database or use an existing user table entry with appropriate role.

## PDF receipts

- mPDF is included under `admin/inc/mpdf/vendor`. Booking receipts can be generated via `generate_pdf.php` (and admin equivalent), which uses mPDF to output booking receipts.

## Git / Common issues

- If `git commit` fails with an index lock error ("Unable to create .git/index.lock"), remove the stale lock file and retry:

```powershell
Remove-Item -Force .git\index.lock
```

- If registration or image upload fails with `Call to undefined function imagecreatefromjpeg()`, enable the GD extension in your PHP `php.ini` (look for `extension=gd`), then restart Apache.

## Troubleshooting AJAX / Registration

- When an AJAX call fails (registration, login, profile upload), open DevTools → Network and inspect the request to `ajax/login_register.php` or `ajax/profile.php` for the server error message and PHP stack trace.
- Server responses often include fatal error messages that point to missing PHP extensions or misconfigurations.

## Security notes

- Do NOT commit API keys (Paytm, SendGrid) to the repository. Keep them private and consider using environment-specific configuration (or a local `config.local.php` not tracked by git).
- Review uploaded files handling and image directories before deploying to production.

## Helpful files to inspect

- Database schema / seed: [DATABASE_FILE/hbwebsite.sql](DATABASE_FILE/hbwebsite.sql#L1)
- Site config and constants: [admin/inc/essentials.php](admin/inc/essentials.php#L1)
- DB connection: [admin/inc/db_config.php](admin/inc/db_config.php#L1)
- AJAX registration and login flow: [ajax/login_register.php](ajax/login_register.php#L1)
- Paytm integration: [inc/paytm/config_paytm.php](inc/paytm/config_paytm.php#L1) and [inc/paytm/encdec_paytm.php](inc/paytm/encdec_paytm.php#L1)
- mPDF vendor: [admin/inc/mpdf/vendor](admin/inc/mpdf/vendor#L1)

## Running locally (example)

1. Place project in `C:/xampp/htdocs/hbwebsite`.
2. Import `DATABASE_FILE/hbwebsite.sql` into MySQL.
3. Edit `admin/inc/db_config.php` and `admin/inc/essentials.php`.
4. Start Apache & MySQL from XAMPP control panel.
5. Visit `http://127.0.0.1/hbwebsite/` (or your `SITE_URL`).

## Support / Next steps

If you'd like, I can:

- Add a `.env` loader and remove API keys from `essentials.php`.
- Add setup scripts or a small checklist script to verify required PHP extensions.
- Create a small CONTRIBUTING or deployment guide.

---
Generated from repository files and inline comments; update values in `admin/inc/essentials.php` and `admin/inc/db_config.php` to match your environment.
