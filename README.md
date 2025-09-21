# Flood Vulnerability Analysis, Thailand
## **Topic:** วิเคราะห์หาจังหวัดเฝ้าระวังน้ำท่วมในประเทศไทยจากข้อมูลสถิติย้อนหลังในประเทศ
## **Group Name:** น้ำจะท่วมแล้วใครจะรอด   
## **Group Members:** 
1. 6720422011 มาวิน บูชา
2. 6720422006 ฐิติกมล การเนตร
3. 6720422019 ณิชาลักษณ์ สวัสดิสรณ์
________________________________________________________________________________________________
## Source of Data
- **ปริมาณน้ำฝน (2000–2025):**  
  ข้อมูลปริมาณน้ำฝนเฉลี่ยรายเดือน (หน่วย: mm) จาก **CHIRPS**  
  (*Climate Hazards Group InfraRed Precipitation with Station data*)  
- **Flood Susceptible:**  
  ข้อมูลจำนวนหมู่บ้านที่อยู่ในแนวร่องน้ำ เสี่ยงภัยน้ำท่วม จาก [www.mitrearth.org](http://www.mitrearth.org)  
- **Recurrent Flood (10 ปี):**  
  ข้อมูลตำบลและพื้นที่ (ไร่) ที่เกิดน้ำท่วมซ้ำซาก 8–10 ครั้ง ระหว่างปี 2015–2025  [กรมพัฒนาที่ดิน](https://gdcatalog.go.th/dataset/gdpublish-lpd04)
- **Province/Region Reference:**  
  ข้อมูล PCODE จังหวัดและภาค ใช้เป็นรหัสอ้างอิงมาตรฐาน  [HDX-TH admin boundaries](https://data.humdata.org/dataset/cod-ab-tha)
_________________________________________________________________________________________________
## Research Questions
1. ฤดูมรสุมของแต่ละภาคในประเทศไทยอยู่ในช่วงเดือนไหนถึงเดือนไหน?  
2. จังหวัดใดที่ควรถูกจัดให้อยู่ใน **watchlist** สำหรับเฝ้าระวังน้ำท่วม?  
3. เมื่อนำปัจจัยอื่น เช่นข้อมูล **น้ำท่วมซ้ำซาก** และ **จำนวนพื้นที่เสี่ยงน้ำหลาก** มาวิเคราะห์ร่วม, จังหวัดใดที่มี **ความเปราะบางสูงสุด** และควรได้รับการจัดการล่วงหน้า?
_________________________________________________________________________________________________
## Data Journey

### Step 1: Rainfall Analysis
เราใช้ข้อมูลปริมาณน้ำฝนรายจังหวัดตั้งแต่ปี 2000–2023 มาคำนวณค่าเฉลี่ยรายเดือน (rfh) และจัดกลุ่มตามภาค เพื่อหาค่า **Peak Rainfall** ของแต่ละภาค 
จากนั้นใช้เกณฑ์ ≥60% ของค่าฝนสูงสุดเพื่อกำหนด *Watchlist Window* หรือช่วงเดือนที่ควรเฝ้าระวังน้ำท่วม เหตุผลที่เลือก 60% เพราะฤดูมรสุมไม่ได้จำกัดอยู่เพียงเดือนเดียวแต่เป็นช่วงต่อเนื่องหลายเดือน
<img width="1093" height="589" alt="image" src="https://github.com/user-attachments/assets/c41b8dcb-9383-4da3-838c-3c20813499d2" />
<sub>Figure1: Average Monthly Rainfall by Region(Heatmap)</sub><br><br>
จาก **Figure 1** สามารถสรุปได้ว่า การกระจายของปริมาณฝนเฉลี่ยรายเดือนสะท้อนถึงช่วงเวลาการเกิดฤดูมรสุมที่แตกต่างกันในแต่ละภูมิภาค ซึ่งมีผลโดยตรงต่อความเสี่ยงด้านอุทกภัย
 - **ภาคเหนือและภาคตะวันออกเฉียงเหนือ** ฤดูมรสุมเริ่มต้นเร็ว โดยมีฝนตกชุกตั้งแต่เดือนมิถุนายนถึงกันยายน ทำให้พื้นที่เหล่านี้ควรได้รับการติดตามสถานการณ์น้ำตั้งแต่ต้นฤดูฝน
 - **ภาคกลาง** มีความเสี่ยงสูงสุดในช่วงเดือนสิงหาคมถึงตุลาคม เนื่องจากเป็นช่วงที่มีฝนตกหนาแน่นและต่อเนื่องมากที่สุด
 - **ภาคใต้** มีฤดูมรสุมยาวนานกว่าภูมิภาคอื่น โดยมีฝนตกต่อเนื่องตั้งแต่ตุลาคมถึงธันวาคม ซึ่งเพิ่มความเสี่ยงต่อการเกิดน้ำท่วมในช่วงปลายปี

<img width="1269" height="245" alt="image" src="https://github.com/user-attachments/assets/22894240-358a-47a1-baaf-fdc95356d793" />
<sub>Table 1: Average Monthly Rainfall by Region</sub><br><br>

**Insight:**
- ภาคเหนือ/อีสาน → ฤดูฝนเริ่มเร็ว (มิ.ย.–ก.ย.)  
- ภาคกลาง → เสี่ยงมากช่วง (ส.ค.–ต.ค.)  
- ภาคใต้ → ฝนลากยาว (ต.ค.–ธ.ค.)

---

### Step 2: Provincial Rainfall - วิเคราะห์ระดับจังหวัด

ต่อมาได้มีการเจาะลึกถึงระดับจังหวัด โดยสร้าง **Heatmap ปริมาณฝนเฉลี่ยรายเดือน** ของแต่ละจังหวัดพร้อมทั้งจัดอันดับ **Top 5 จังหวัดที่มีปริมาณฝนรวมสูงสุดในแต่ละภูมิภาค**  
การวิเคราะห์นี้ช่วยให้เห็นภาพความแตกต่างของ **ความเสี่ยงด้านฝนและอุทกภัย** ในแต่ละพื้นที่ได้อย่างชัดเจนซึ่งเป็นข้อมูลสำคัญสำหรับการวางแผนและเฝ้าระวังน้ำท่วมในเชิงพื้นที่

### **Central Region**
<img width="1258" height="748" alt="image" src="https://github.com/user-attachments/assets/fbd3ec57-0143-4af1-ac5a-e5487ac36e90" />
<sub>Figure 2: Average Monthly Rainfall in the Central Region and Provinces with the Highest Rainfall(Heatmap)</sub><br><br>

**Top 5 จังหวัดที่มีปริมาณฝนสูงสุดในภาคกลาง**: นครนายก, กรุงเทพมหานคร, สมุทรปราการ, สมุทรสาคร และนนทบุรี  

### **East Region**
<img width="1225" height="638" alt="image" src="https://github.com/user-attachments/assets/110d99e5-50f0-4021-b2fa-7684b18e4687" />
<sub>Figure 3: Average Monthly Rainfall in the East Region and Provinces with the Highest Rainfall(Heatmap)</sub><br><br>

**Top 5 จังหวัดที่มีปริมาณฝนสูงสุดในภาคตะวันออก**: ตราด, จันทบุรี, ปราจีนบุรี, ระยอง และฉะเชิงเทรา  

### **Northeast Region**
<img width="1254" height="1048" alt="image" src="https://github.com/user-attachments/assets/43f351a3-3d28-453b-9325-7637265ca7cd" />
<sub>Figure 4: Average Monthly Rainfall in the Northeast Region and Provinces with the Highest Rainfall(Heatmap)</sub><br><br>
Top 5 Provinces with Highest Rainfall in Northeast: Bueng Kan, Nakhon Phanom, Nong Khai, Ubon Ratchathani, Sakon Nakhon

### **North Region**
<img width="1243" height="938" alt="image" src="https://github.com/user-attachments/assets/9463c92b-7ffa-4ca4-bc80-0c993393bb8c" />
<sub>Figure 5: Average Monthly Rainfall in the North Region and Provinces with the Highest Rainfall(Heatmap)</sub><br><br>
Top 5 Provinces with Highest Rainfall in North: Chiang Rai, Nan, Phayao, Uthai Thani, Tak

### **South Region**
<img width="1240" height="804" alt="image" src="https://github.com/user-attachments/assets/5fc29272-4e6b-4bf3-aa49-1a81c32f8edc" />
<sub>Figure 6: Average Monthly Rainfall in the South Region and Provinces with the Highest Rainfall(Heatmap)</sub><br><br>
Top 5 Provinces with Highest Rainfall in South: Ranong, Phangnga, Narathiwat, Yala, Phuket

### **West Region**
<img width="1235" height="755" alt="image" src="https://github.com/user-attachments/assets/3ec1e0e5-f61a-4ed6-91ad-619600fb7f10" />
<sub>Figure 7: Average Monthly Rainfall in the West Region and Provinces with the Highest Rainfall(Heatmap)</sub><br><br>
Top 5 Provinces with Highest Rainfall in West: Prachuap Khiri Khan, Kanchanaburi, Ratchaburi, Phetchaburi, Suphan Buri

**Insight:**  
- **จังหวัดต้นน้ำ** เช่น **เชียงใหม่, เชียงราย และอุบลราชธานี**  
  มักเผชิญกับฝนตกหนักต่อเนื่อง ทำให้มีความเสี่ยงสูงที่จะเป็น **จุดเริ่มต้นของน้ำหลาก (upstream floods)**  
  ซึ่งสามารถกระทบพื้นที่ลุ่มน้ำตอนล่างได้โดยตรง  

- **จังหวัดลุ่มเจ้าพระยา** เช่น **นครสวรรค์ และพระนครศรีอยุธยา**  
  แม้จะไม่ได้มีปริมาณฝนสูงสุด แต่ยังคงมีความเสี่ยงจาก **น้ำสะสมที่ไหลมาจากตอนบน (downstream impact)**  
  ส่งผลให้เกิดน้ำท่วมใหญ่ได้หากมีการระบายน้ำไม่เพียงพอ
  
---

### Step 3: Flood Risk Factors — รวมปัจจัยเสี่ยง
เพื่อเพิ่มมิติในการวิเคราะห์ เราเสริมข้อมูลอีกสองปัจจัยคือ **Recurrent Flood Area (10Y)** และ **Flood Susceptible Sites** มาประกอบกับปริมาณฝนใน *watchlist months* 
โดย Normalize ค่าทุกปัจจัยให้อยู่ในช่วง 0–1 จากนั้นรวมเป็น **Composite Score** ด้วยน้ำหนัก: 
- **Rainfall in Watchlist Months** (50%)  
- **Recurrent Flood Area (10Y)** (30%)  
- **Flood Susceptible Sites** (20%)  

**Central**
<img width="1412" height="322" alt="image" src="https://github.com/user-attachments/assets/aea28b51-44cc-4116-b733-15be4827885e" />


**East**
<img width="1865" height="175" alt="image" src="https://github.com/user-attachments/assets/92005764-5a84-4dd4-a249-7b158b9c5f5d" />

**Northeast**
<img width="1412" height="452" alt="image" src="https://github.com/user-attachments/assets/5eb641c2-38b8-4092-9222-a4ac9f3faced" />

**North**
<img width="1408" height="389" alt="image" src="https://github.com/user-attachments/assets/39632e29-d2e1-4e39-afe2-2a00cb85ddf4" />

**South**
<img width="1408" height="319" alt="image" src="https://github.com/user-attachments/assets/ab8ee1d4-31b4-400b-a6a4-50f74ee2ef67" />

**West**
<img width="1412" height="131" alt="image" src="https://github.com/user-attachments/assets/0d92e970-97e0-4ae8-a008-a267486baa75" />

จากนั้น เราทำ spider chart เพื่อดูว่า แต่ละภาคนั้นมีปัจจัยใดที่เป็นปัจจัยหลักในการทำให้เสี่ยงน้ำท่วม
<img width="2006" height="1146" alt="image" src="https://github.com/user-attachments/assets/4b5ab980-6a5d-4fb7-95f9-1b07db9d3eae" />


ซึ่งจะเห็นว่าจากกราฟนั้น
ภาคเหนือ/อีสาน → ค่าฝน (Rainfall) สูงสุด → เสี่ยงจากปริมาณฝนโดยตรง
ภาคกลาง → Recurrent Flood area สูง → เสี่ยงจากน้ำท่วมซ้ำซาก
กรุงเทพฯ และปริมณฑล (Central) → Susceptible sites สูง → เสี่ยงจากความหนาแน่นของพื้นที่อ่อนไหว
ภาคใต้ → ฝนยาวนาน แต่ recurrent flood และ susceptible อาจไม่สูงเท่า


---

### Step 4: Composite Vulnerability
- จัดอันดับ Top 10 จังหวัดเสี่ยงน้ำท่วมจากการคำนวน composite score 3ปัจจัยหลักในแต่ละภาค
<img width="2052" height="1666" alt="image" src="https://github.com/user-attachments/assets/9bbb2bd4-dc78-4779-89dc-9791b40609ae" />




**Insight:**  
- ภาคเหนือ/อีสาน → เสี่ยงจากฝนมาก  
- ภาคกลาง → เสี่ยงจากพื้นที่น้ำท่วมซ้ำซากขนาดใหญ่ (อยุธยา, สุพรรณบุรี, อ่างทอง)  
- กรุงเทพฯ/ปริมณฑล → เสี่ยงจาก **susceptible sites** แม้ฝนไม่สูงสุด

  ---

# Summary Top 10 Provinces by Composite Flood Vulnerability (by Region)


## สรุปภาพรวม
- **เหนือ–อีสาน:** เสี่ยงจาก **ฝนหนัก** → ต้องเตือน upstream flood & landslide  
- **ภาคกลาง:** เสี่ยงจาก **น้ำท่วมซ้ำซาก + susceptible sites** → ควรเน้นจัดการลุ่มเจ้าพระยาและโครงสร้างเมือง  
- **ภาคใต้:** เสี่ยงจาก **ฝนตกลากยาว** → ต้องมีระบบ early warning & drainage ที่ยืดหยุ่น  

# **Answering the Research Question**
## ฤดูมรสุมของแต่ละภาคในประเทศไทยอยู่ในช่วงเดือนไหนถึงเดือนไหน?
**ฤดูมรสุม (Watchlist):**  
  - เหนือ: มิ.ย.–ก.ย.  
  - อีสาน: ก.ค.–ก.ย.  
  - กลาง: ส.ค.–ต.ค.  
  - ใต้: ต.ค.–ธ.ค.  

## จังหวัดใดที่ควรถูกจัดให้อยู่ใน watchlist สำหรับเฝ้าระวังน้ำท่วม?
**จังหวัดเฝ้าระวัง (Watchlist Provinces):**  
  - เหนือ: เชียงใหม่, เชียงราย  
  - อีสาน: อุบลราชธานี, โคราช  
  - กลาง: อยุธยา, สุพรรณบุรี  
  - ใต้: นครศรีธรรมราช, สงขลา  

## เมื่อนำปัจจัยอื่น เช่นข้อมูล น้ำท่วมซ้ำซาก และ จำนวนพื้นที่เสี่ยงน้ำหลาก มาวิเคราะห์ร่วม, จังหวัดใดที่มี ความเปราะบางสูงสุด และควรได้รับการจัดการล่วงหน้า?
**จังหวัดเปราะบางที่สุด (Composite Score สูงสุด):**  

**ภาคเหนือ (North)**
  **จุดเด่น:** ความเสี่ยงหลักมาจาก **ฝนตกหนักในช่วงมรสุม** โดยเฉพาะจังหวัดต้นน้ำ  
  - **เชียงใหม่, เชียงราย** → ฝนสะสมสูง เสี่ยงน้ำหลาก/ดินถล่ม  
  - **น่าน, พะเยา** → runoff จากพื้นที่ภูเขา  

  **Insight:** ภาคเหนือเสี่ยงจาก “**ฝนสะสม**” มากกว่าปัจจัยจากพื้นที่น้ำท่วมซ้ำซาก  

**ภาคตะวันออกเฉียงเหนือ (Northeast)**
  **จุดเด่น:** เสี่ยงทั้งจาก **ฝนสูง** และ **พื้นที่น้ำท่วมซ้ำซาก** ในลุ่มน้ำโขงและชี–มูล  

  - **อุบลราชธานี** → downstream ของแม่น้ำมูล  
  - **นครราชสีมา (โคราช)** → เส้นทางรับน้ำจากต้นน้ำ  
  - **ขอนแก่น, ร้อยเอ็ด** → recurrent flood + แอ่งน้ำขัง  

  **Insight:** ภาคอีสานเสี่ยงจาก **“ฝนหนักและมีพื้นที่น้ำท่วมซ้ำซาก”** ผสมกัน  

**ภาคกลาง (Central)**
  **จุดเด่น:** เสี่ยงจาก **recurrent flood area** และ **susceptible sites** มากกว่าฝนโดยตรง  

  - **พระนครศรีอยุธยา, สุพรรณบุรี** → พื้นที่ท่วมซ้ำซากใหญ่  
  - **อ่างทอง, นครสวรรค์** → ได้รับผลสะสมจาก upstream  
  - **กรุงเทพฯ** → susceptible sites หนาแน่น แม้ฝนไม่สูงสุด  

  **Insight:** ภาคกลางเสี่ยงจาก “**มีพื้นที่น้ำท่วมซ้ำซาก**” และ “**โครงสร้างของเมืองหนาแน่น**”  


**ภาคใต้ (South)**
  **จุดเด่น:** **ฝนตกต่อเนื่องยาวนาน (ต.ค.–ธ.ค.)** ทำให้ท่วมซ้ำบ่อย  

  - **นครศรีธรรมราช, สงขลา** → ฝนตกหนักต่อเนื่อง น้ำท่วมเมือง  
  - **พัทลุง, สุราษฎร์ธานี** → ฝนสะสมสูง ไหลลงทะเล  

  **Insight:** ภาคใต้เสี่ยงจาก “**ฝนยาวนาน**” และ “**ฝนตกหนักซ้ำหลายรอบ**”  

---

## Practical Use
- ใช้กำหนด **จังหวัดเป้าหมายของ Early Warning** ในแต่ละภาค  
- วางแผนทรัพยากรรับมือ:  
  - เสริมคันกั้นน้ำลุ่มเจ้าพระยา  
  - พัฒนาระบบระบายน้ำในภาคต่างๆ 
  - เตรียมการอพยพและช่วยเหลือจังหวัดเสี่ยง upstream  

---


