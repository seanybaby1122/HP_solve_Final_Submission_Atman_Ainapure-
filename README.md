# HP_solve_Final_Submission_Atman_Ainapure-
HP Solve 2023

3oth May and 31st May data results.xlsx is the web scraped data

IPYNB is the jupyter notebook in which the cleaned the data and performed sentiment analysis. I made the knowledge graph in it too.

cleaned_final_output.xlsx is final output on which I made the knowledge graph.

knowledge_graph.graphml is the knowledge graph.

links_printer_laptop.xlsx is the excel file in which all the web scraped links are stored.

Power Automate workflows pdf file contains the workflows which I used for web scraping.


Tech Stack Used:
No code solution for webscraping
Python for data cleaning, sentiment analysis and creation of knowledge graph.
from flask import Flask, request, jsonify
from werkzeug.security import generate_password_hash, check_password_hash
from functools import wraps

app = Flask(__name__)

# In-memory user storage for demonstration
users = {
    "admin": {
        "password": generate_password_hash("adminpass"),
        "role": "admin"
    },
    "user": {
        "password": generate_password_hash("userpass"),
        "role": "user"
    }
}

def role_required(required_role):
    def decorator(f):
        @wraps(f)
        def decorated_function(*args, **kwargs):
            auth = request.authorization
            if not auth or auth.username not in users or not check_password_hash(users[auth.username]["password"], auth.password):
                return jsonify({"message": "Authentication required"}), 401
            if users[auth.username]["role"] != required_role:
                return jsonify({"message": "Insufficient permissions"}), 403
            return f(*args, **kwargs)
        return decorated_function
    return decorator

@app.route('/admin', methods=['GET'])
@role_required("admin")
def admin_only():
    return jsonify({"message": "Welcome, admin!"})

if __name__ == '__main__':
    app.run(debug=True)
