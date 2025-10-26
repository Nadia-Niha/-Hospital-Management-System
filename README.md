# app.py
from flask import Flask, render_template_string

app = Flask(__name__)

# Mock Data
patients = [
    {"name": "John Doe", "id": "P001", "history": ["01 Sep 2025: Blood Test", "15 Aug 2025: Consultation"]},
    {"name": "Mary Smith", "id": "P002", "history": ["20 Sep 2025: MRI Scan"]}
]

appointments = [
    {"patient": "John Doe", "doctor": "Dr. Smith", "datetime": "26 Oct 2025 10:00 AM", "type": "Check-up"},
    {"patient": "Mary Smith", "doctor": "Dr. Lee", "datetime": "28 Oct 2025 02:00 PM", "type": "Consultation"}
]

admin_dashboard = {
    "total_patients": len(patients),
    "total_appointments": len(appointments),
    "total_revenue": "$45,230"
}

available_slots = {
    "Mon 26 Oct": "09:00-12:00",
    "Tue 27 Oct": "10:00-02:00"
}

notifications = [
    "26 Oct 2025 10:00 AM: John Doe booked an appointment",
    "28 Oct 2025 02:00 PM: Mary Smith cancelled an appointment"
]

# HTML Template
template = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hospital Management System</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body { padding: 20px; background-color: #f8f9fa; }
        .card { margin-bottom: 20px; }
        .section-title { margin-top: 30px; margin-bottom: 15px; }
        .scroll-table { max-height: 200px; overflow-y: auto; }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="text-center mb-4">🏥 Hospital Management System</h1>

        <!-- Admin Dashboard -->
        <h3 class="section-title">Admin Analytics Dashboard</h3>
        <div class="row">
            <div class="col-md-4">
                <div class="card text-bg-primary">
                    <div class="card-body">
                        <h5 class="card-title">Total Patients</h5>
                        <p class="card-text fs-3">{{ admin.total_patients }}</p>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="card text-bg-success">
                    <div class="card-body">
                        <h5 class="card-title">Total Appointments</h5>
                        <p class="card-text fs-3">{{ admin.total_appointments }}</p>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="card text-bg-warning">
                    <div class="card-body">
                        <h5 class="card-title">Total Revenue</h5>
                        <p class="card-text fs-3">{{ admin.total_revenue }}</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- Patient Portal -->
        <h3 class="section-title">🧑‍⚕️ Patient Portal</h3>
        <div class="card">
            <div class="card-body">
                <h5>Upcoming Appointments</h5>
                <ul>
                    {% for a in appointments %}
                        <li>{{ a.doctor }} - {{ a.datetime }}</li>
                    {% endfor %}
                </ul>
                <h5>Medical History</h5>
                {% for p in patients %}
                    <strong>{{ p.name }}</strong>
                    <ul>
                        {% for h in p.history %}
                            <li>{{ h }}</li>
                        {% endfor %}
                    </ul>
                {% endfor %}
                <h5>Notifications</h5>
                <ul>
                    {% for n in notifications %}
                        <li>{{ n }}</li>
                    {% endfor %}
                </ul>
            </div>
        </div>

        <!-- Doctor Dashboard -->
        <h3 class="section-title">🩺 Doctor Dashboard</h3>
        <div class="card">
            <div class="card-body scroll-table">
                <h5>Daily Appointments</h5>
                <table class="table table-striped">
                    <thead>
                        <tr>
                            <th>Patient Name</th>
                            <th>Time</th>
                            <th>Appointment Type</th>
                        </tr>
                    </thead>
                    <tbody>
                        {% for a in appointments %}
                        <tr>
                            <td>{{ a.patient }}</td>
                            <td>{{ a.datetime.split(' ')[-2] + ' ' + a.datetime.split(' ')[-1] }}</td>
                            <td>{{ a.type }}</td>
                        </tr>
                        {% endfor %}
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Appointment Management -->
        <h3 class="section-title">📅 Appointment Management</h3>
        <div class="card">
            <div class="card-body">
                <h5>Available Slots</h5>
                <ul>
                    {% for day, slot in slots.items() %}
                        <li>{{ day }}: {{ slot }}</li>
                    {% endfor %}
                </ul>
                <button class="btn btn-primary">Book Appointment</button>
                <button class="btn btn-danger">Cancel Appointment</button>
            </div>
        </div>

        <!-- Notifications -->
        <h3 class="section-title">🔔 Notifications & Alerts</h3>
        <div class="card">
            <div class="card-body">
                <ul>
                    {% for n in notifications %}
                        <li>{{ n }}</li>
                    {% endfor %}
                </ul>
            </div>
        </div>

    </div>
</body>
</html>
"""

# Routes
@app.route('/')
def home():
    return render_template_string(template, 
                                  admin=admin_dashboard, 
                                  patients=patients, 
                                  appointments=appointments,
                                  slots=available_slots,
                                  notifications=notifications)

# Run the App
if __name__ == '__main__':
    app.run(debug=True)
