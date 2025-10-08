## Remote IT Consulting - Static Web App

This is a static, mobile-first marketing site for a remote IT consulting firm. It is designed to be deployed to Azure App Service or Azure Static Web Apps.

### Local Development

- Open `index.html` in your browser, or use a simple HTTP server:

```bash
# Python 3
python -m http.server 8080
# Node
npx serve . -l 8080 --single
```

### Project Structure

```
.
├── index.html
├── assets/
│   ├── logo.svg
│   └── favicon.svg
├── css/
│   └── styles.css
└── js/
    └── main.js
```

### Deploy to Azure App Service (Linux)

1. Create a resource group (skip if you have one):
   ```bash
   az group create --name rg-itconsulting --location westus
   ```
2. Create a storage-backed Linux App Service plan and web app:
   ```bash
   az appservice plan create --name asp-itconsulting --resource-group rg-itconsulting --sku B1 --is-linux
   az webapp create --resource-group rg-itconsulting --plan asp-itconsulting --name <YOUR_UNIQUE_APP_NAME> --runtime "NODE:20-lts"
   ```
   Note: Runtime is arbitrary for static sites; Node is convenient.
3. Configure static file serving (optional but recommended):
   ```bash
   az webapp config appsettings set \
     --resource-group rg-itconsulting \
     --name <YOUR_UNIQUE_APP_NAME> \
     --settings WEBSITE_RUN_FROM_PACKAGE=1
   ```
4. Deploy zipped site contents:
   ```bash
   zip -r site.zip index.html css js assets
   az webapp deploy --resource-group rg-itconsulting --name <YOUR_UNIQUE_APP_NAME> --src-path site.zip --type zip
   ```
5. Browse your site:
   ```bash
   az webapp browse --resource-group rg-itconsulting --name <YOUR_UNIQUE_APP_NAME>
   ```

### Deploy to Azure Static Web Apps (alternative)

```bash
az staticwebapp create \
  --name swa-itconsulting \
  --resource-group rg-itconsulting \
  --source . \
  --location westus2 \
  --output-location .
```

### Custom Domain and HTTPS

- Configure in your Web App's `Custom domains` blade; add TXT then CNAME/ALIAS.
- Enable `HTTPS Only` for automatic TLS.

### CI/CD (optional GitHub Actions)

- Push this repo to GitHub.
- For App Service, use the `Deployment Center` to generate a workflow.
- For Static Web Apps, the `create` command will set up Actions automatically.


