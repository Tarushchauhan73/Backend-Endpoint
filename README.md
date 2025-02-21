# Backend-Endpoint
from flask import Flask, request, jsonify

app = Flask(__name__)

# Hardcoded user details
USER_ID = "john_doe_17091999"
EMAIL = "john@xyz.com"
ROLL_NUMBER = "ABCD123"
OPERATION_CODE = 1

# POST /bfhl
@app.route('/bfhl', methods=['POST'])
def post_bfhl():
    try:
        data = request.json.get('data', [])
        numbers = []
        alphabets = []
        highest_alphabet = None

        for item in data:
            if isinstance(item, str):
                if item.isalpha():
                    alphabets.append(item)
                    if highest_alphabet is None or item.lower() > highest_alphabet.lower():
                        highest_alphabet = item
                elif item.isdigit():
                    numbers.append(item)

        response = {
            "is_success": True,
            "user_id": USER_ID,
            "email": EMAIL,
            "roll_number": ROLL_NUMBER,
            "numbers": numbers,
            "alphabets": alphabets,
            "highest_alphabet": [highest_alphabet] if highest_alphabet else []
        }
        return jsonify(response), 200
    except Exception as e:
        return jsonify({"is_success": False, "error": str(e)}), 400

# GET /bfhl
@app.route('/bfhl', methods=['GET'])
def get_bfhl():
    return jsonify({"operation_code": OPERATION_CODE}), 200

# Run the app
if __name__ == '__main__':
    app.run(debug=True)
