FROM node:20

# Set direktori kerja
WORKDIR /app

# Salin file dependency terlebih dahulu
COPY package*.json ./

# Install dependency
RUN npm install

# Salin semua file ke dalam container
COPY . .

# Jalankan React dev server
CMD ["npm", "start"]
