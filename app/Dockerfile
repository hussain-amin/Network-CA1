# Use the official lightweight Nginx image
FROM nginx:latest

# Copy your sample HTML file into the default Nginx web root directory
# Assuming your HTML file is named index.html in the local 'app' directory
COPY . /usr/share/nginx/html

# Expose the default HTTP port 80. 
# This matches the port we opened in the Azure Network Security Group (NSG) in Terraform.
EXPOSE 80

# Nginx default configuration already handles the web server startup (CMD command), 
# so no custom CMD instruction is needed.
