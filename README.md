<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>CRES Lessee</title>
  <!-- Supabase Library -->
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }
    body { background-color: #111827; color: white; display: flex; flex-direction: column; height: 100vh; }
    
    /* Header */
    .header { display: flex; justify-content: space-between; align-items: center; padding: 20px; background-color: #1F2937; border-bottom: 1px solid #374151; }
    .logo { font-size: 20px; font-weight: bold; }
    .logo span { color: #F59E0B; }
    .user { color: #9CA3AF; font-size: 14px; }

    /* Main Content */
    .main-content { flex: 1; padding: 20px; overflow-y: auto; padding-bottom: 80px; }
    .card { background-color: #1F2937; border-radius: 12px; padding: 20px; border: 1px solid #374151; }
    .card-title { text-align: center; font-size: 18px; font-weight: bold; margin-bottom: 20px; border-bottom: 2px solid #F59E0B; padding-bottom: 10px; display: inline-block; width: 100%; }
    
    /* Form */
    .form-group { margin-bottom: 15px; }
    label { display: block; text-align: center; color: #9CA3AF; font-size: 14px; margin-bottom: 8px; }
    input, select { width: 100%; background-color: #111827; border: 1px solid #374151; color: white; padding: 12px; border-radius: 8px; font-size: 16px; outline: none; }
    input:focus, select:focus { border-color: #F59E0B; }
    button { width: 100%; background-color: #F59E0B; color: #111827; font-weight: bold; padding: 15px; border: none; border-radius: 8px; font-size: 16px; margin-top: 10px; cursor: pointer; }
    
    /* Bottom Nav */
    .bottom-nav { position: fixed; bottom: 0; width: 100%; background-color: #1F2937; display: flex; justify-content: space-around; padding: 15px 0; border-top: 1px solid #374151; }
    .nav-item { color: #6B7280; font-size: 12px; text-align: center; text-decoration: none; }
    .nav-item.active { color: #F59E0B; font-weight: bold; }
  </style>
</head>
<body>

  <div class="header">
    <div class="logo">CRES <span>Lessee</span></div>
    <div class="user">Abebe B.</div>
  </div>

  <div class="main-content">
    <div class="card">
      <div class="card-title">Driver Registration</div>
      
      <div class="form-group">
        <label>Full Legal Name</label>
        <input type="text" id="fullName" placeholder="e.g. Abebe Bikila">
      </div>

      <div class="form-group">
        <label>Phone Number</label>
        <input type="tel" id="phone" placeholder="0911-223-344">
      </div>

      <div class="form-group">
        <label>Driver's License Grade</label>
        <select id="licenseGrade">
          <option value="Grade 1">Grade 1 (Public Service)</option>
          <option value="Grade 2">Grade 2 (Commercial)</option>
          <option value="Grade 3">Grade 3 (Private)</option>
        </select>
      </div>

      <div class="form-group">
        <label>Advance Payment Readiness</label>
        <select id="advancePayment">
          <option value="Standard (2 Months)">Standard (2 Months Advance)</option>
          <option value="1 Month">1 Month Advance</option>
        </select>
      </div>

      <button onclick="submitRegistration()">Submit Registration</button>
    </div>
  </div>

  <div class="bottom-nav">
    <a href="#" class="nav-item active">Register</a>
    <a href="#" class="nav-item">My Lease</a>
    <a href="#" class="nav-item">Score</a>
    <a href="#" class="nav-item">Profile</a>
  </div>

  <script>
    // --- REPLACE THESE WITH YOUR SUPABASE KEYS ---
    const supabaseUrl = 'YOUR_SUPABASE_URL_HERE';
    const supabaseKey = 'YOUR_SUPABASE_ANON_KEY_HERE';
    
    // Initialize Supabase
    const supabase = window.supabase.createClient(supabaseUrl, supabaseKey);

    async function submitRegistration() {
      const name = document.getElementById('fullName').value;
      const phone = document.getElementById('phone').value;
      const license = document.getElementById('licenseGrade').value;
      const payment = document.getElementById('advancePayment').value;

      if(!name || !phone) {
        alert("Please fill in all fields.");
        return;
      }

      // Save to the unified 'requests' table we created earlier
      const { data, error } = await supabase
        .from('requests')
        .insert([
          { 
            user_phone: phone, 
            request_type: 'DRIVER', 
            status: 'PENDING',
            data: { 
              name: name, 
              licenseGrade: license, 
              advancePayment: payment 
            } 
          }
        ]);

      if (error) {
        alert("Error submitting: " + error.message);
      } else {
        alert("Registration Submitted! Waiting for Admin Approval.");
        // Clear form
        document.getElementById('fullName').value = '';
        document.getElementById('phone').value = '';
      }
    }
  </script>
</body>
</html>
