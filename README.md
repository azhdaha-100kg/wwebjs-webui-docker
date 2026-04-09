# Deployment Instructions for wwebjs-webui-docker

## Heroku Deployment
1. **Create a New Heroku App:**
   ```bash
   heroku create your-app-name
   ```
2. **Set Environment Variables:**
   ```bash
   heroku config:set VARIABLE_NAME=value
   ```
   (Replace `VARIABLE_NAME` with your specific environment variables)
3. **Deploy Your App:**
   ```bash
   git push heroku main
   ```

## Docker on VPS Deployment
1. **Install Docker:**
   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   sh get-docker.sh
   ```
2. **Clone the Repository:**
   ```bash
   git clone https://github.com/azhdaha-100kg/wwebjs-webui-docker.git
   cd wwebjs-webui-docker
   ```
3. **Build the Docker Image:**
   ```bash
   docker build -t your-image-name .
   ```
4. **Run the Container:**
   ```bash
   docker run -d -p 80:80 -e VARIABLE_NAME=value your-image-name
   ```

## Local Testing
1. **Clone the Repository:**
   ```bash
   git clone https://github.com/azhdaha-100kg/wwebjs-webui-docker.git
   cd wwebjs-webui-docker
   ```
2. **Install Dependencies:**
   ```bash
   npm install
   ```
3. **Run the Application:**
   ```bash
   npm start
   ```

## Environment Variables
- `DATABASE_URL`: URL for your database
- `PORT`: Port for the application to run on (default is 3000)

## Troubleshooting Tips
- Ensure all environment variables are correctly set and available in the environment where the application is running.
- Check the logs for any errors by running:
   ```bash
   heroku logs --tail
   ```
   (for Heroku deployments)
- For Docker, view logs with:
   ```bash
   docker logs container_id
   ```
- Make sure the required ports are open in your firewall settings if the application is not accessible.

For further details, please refer to the `server.js` configuration and ensure all required settings are in place.
