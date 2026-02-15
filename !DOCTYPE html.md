  
<!DOCTYPE html>  
<html>  
<head>  
<meta charset="UTF-8">  
<title>**ระบบแจ้งปัญหาโรงเรียน**</title>  
</head>  
<body>  
  
<div id="app"></div>  
  
<style>  
@import url('https://fonts.googleapis.com/css2?family=Prompt:wght@400;600;700&display=swap');  
  
*{  
  margin:0;  
  padding:0;  
  box-sizing:border-box;  
  font-family:'Prompt',sans-serif;  
}  
  
body{  
  background:linear-gradient(135deg,#141e30,#243b55);  
  min-height:100vh;  
  display:flex;  
  justify-content:center;  
  align-items:center;  
  color:white;  
}  
  
.container{  
  width:95%;  
  max-width:1100px;  
  text-align:center;  
}  
  
.glass{  
  background:rgba(255,255,255,0.1);  
  backdrop-filter:blur(20px);  
  padding:30px;  
  border-radius:25px;  
}  
  
button{  
  padding:8px 15px;  
  border:none;  
  border-radius:8px;  
  cursor:pointer;  
  margin:5px;  
  font-weight:600;  
}  
  
.blue{background:#0072ff;color:white;}  
.green{background:#2ecc71;color:white;}  
.red{background:#e74c3c;color:white;}  
.gray{background:#95a5a6;color:white;}  
  
input, textarea{  
  width:100%;  
  padding:8px;  
  border-radius:8px;  
  border:none;  
  margin:6px 0;  
}  
  
textarea{  
  height:100px;  
  resize:none;  
}  
  
table{  
  width:100%;  
  background:white;  
  color:#333;  
  border-collapse:collapse;  
  font-size:13px;  
}  
  
th, td{  
  padding:8px;  
  border-bottom:1px solid #ddd;  
}  
  
th{  
  background:#0072ff;  
  color:white;  
}  
  
.status-approved{  
  background:#2ecc71;  
  color:white;  
  font-weight:700;  
}  
  
.status-rejected{  
  background:#e74c3c;  
  color:white;  
  font-weight:700;  
}  
  
.status-pending{  
  background:#f1c40f;  
  color:black;  
  font-weight:600;  
}  
</style>  
  
<script>  
  
const ADMIN_USERNAME = "AOMSIN";  
const ADMIN_PASSWORD = "451122";  
  
let currentUserPhone = "";  
  
function showHome(){  
  document.getElementById("app").innerHTML = `  
  <div class="container">  
    <div class="glass">  
      <h1>**รับแจ้งเหตุปัญหาภายในโรงเรียนพัฒนานิคม**</h1>  
      <button class="blue" onclick="showUserLogin()">**เริ่มส่งปัญหา**</button>  
      <br>  
      <button class="blue" onclick="showAdminLogin()">**เข้าสู่ระบบสภานักเรียน**</button>  
    </div>  
  </div>`;  
}  
  
function showUserLogin(){  
  document.getElementById("app").innerHTML = `  
  <div class="container">  
    <div class="glass">  
      <h2>**เข้าสู่ระบบด้วยเบอร์โทร**</h2>  
      <input id="phone" placeholder="**กรอกเบอร์โทร**">  
      <button class="blue" onclick="loginUser()">**ยืนยัน**</button>  
      <button class="gray" onclick="showHome()">**กลับ**</button>  
    </div>  
  </div>`;  
}  
  
function loginUser(){  
  let phone = document.getElementById("phone").value;  
  if(!phone){ alert("**กรอกเบอร์ก่อน**"); return; }  
  currentUserPhone = phone;  
  showUserDashboard();  
}  
  
function showUserDashboard(){  
  let data = JSON.parse(localStorage.getItem("problems")) || [];  
  let myProblems = data.filter(p=>p.phone===currentUserPhone);  
  
  let rows = "";  
  
  if(myProblems.length===0){  
    rows = `<tr><td colspan="5">**ยังไม่เคยแจ้งปัญหา**</td></tr>`;  
  }else{  
    myProblems.forEach((item,index)=>{  
  
      let displayStatus="**รอยืนยัน**";  
      let statusClass="status-pending";  
  
      if(item.status==="**ยินยอม**"){  
        displayStatus="**ยินยอมแล้ว**";  
        statusClass="status-approved";  
      }else if(item.status==="**ไม่ยินยอม**"){  
        displayStatus="**ไม่ยินยอม**";  
        statusClass="status-rejected";  
      }  
  
      rows+=`  
      <tr>  
        <td>${index+1}</td>  
        <td>${item.fullname}</td>  
        <td>${item.problem}</td>  
        <td>${item.time}</td>  
        <td class="${statusClass}">${displayStatus}</td>  
      </tr>`;  
    });  
  }  
  
  document.getElementById("app").innerHTML=`  
  <div class="container">  
    <div class="glass">  
      <h2>**สถานะการแจ้งปัญหา**</h2>  
      <table>  
        <tr>  
          <th>**ลำดับ**</th>  
          <th>**ชื่อ**</th>  
          <th>**ปัญหา**</th>  
          <th>**เวลา**</th>  
          <th>**สถานะ**</th>  
        </tr>  
        ${rows}  
      </table>  
      <br>  
      <button class="green" onclick="showForm()">**ส่งแจ้งปัญหาเพิ่มเติม**</button>  
      <button class="gray" onclick="showHome()">**ออกจากระบบ**</button>  
    </div>  
  </div>`;  
}  
  
showHome();  
  
</script>  
  
</body>  
</html>  
