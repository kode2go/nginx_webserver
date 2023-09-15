# conf file

```
server {
    listen 80;
    server_name x.x.x.x;

    location /flask {
        proxy_pass http://127.0.0.1:5000;
        include proxy_params;
    }

    location /dj_email {
        proxy_pass http://127.0.0.1:8000;
        include proxy_params;
    }

    location / {
        root /var/www/html;
        index index.html;
    }
}
```

HTML: `sudo nano /var/www/html/index.html`

```
<!DOCTYPE html>
<html>
<head>
    <title>Home</title>
</head>
<body>
    <h1>Welcome to the Combined Site</h1>
    <p>Click on the links below to access the applications:</p>
    <ul>
        <li><a href="/flask">Flask CSV API</a></li>
        <li><a href="/dj_email">Django Email App</a></li>
        <li><a href="/fast">Fast Api</a></li>
    </ul>
</body>
</html>

```

DJ Email:

```
from django.http import HttpResponse

def dj_email(request):
    # Add your view logic here
    return HttpResponse("This is the dj_email view.")

urlpatterns = [
    path('dj_email/', homePage, name='home'),
    path('dj_email/admin/', admin.site.urls),
    path('dj_email/emailsender/', include('emailsender.urls')),
```

Flask:

```
from flask import Flask, jsonify

app = Flask(__name__)

# Replace 'data.csv' with your actual CSV file name and path
CSV_FILE = 'data.csv'

@app.route('/flask', methods=['GET'])
def get_data():
    try:
        with open(CSV_FILE, 'r') as file:
            data = file.read()
        return jsonify({"data": data.splitlines()})
    except FileNotFoundError:
        return jsonify({"error": "CSV file not found"}), 404

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```
