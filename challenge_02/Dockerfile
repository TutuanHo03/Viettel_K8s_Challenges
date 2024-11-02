FROM nginx:latest

# Tạo thư mục chứa website
RUN mkdir /usr/share/nginx/html/my-website

# Sao chép website tĩnh vào thư mục
COPY ./my-website/* /usr/share/nginx/html/my-website/

# Sao chép file cấu hình nginx
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 8080
EXPOSE 8080
