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
<img width="1269" height="245" alt="image" src="https://github.com/user-attachments/assets/22894240-358a-47a1-baaf-fdc95356d793" />




**Insight:**  
- ภาคเหนือ/อีสาน → ฤดูฝนเริ่มเร็ว (มิ.ย.–ก.ย.)  
- ภาคกลาง → เสี่ยงมากช่วง (ส.ค.–ต.ค.)  
- ภาคใต้ → ฝนลากยาว (ต.ค.–ธ.ค.)  




---

### Step 2: Provincial Rainfall - วิเคราะห์ระดับจังหวัด
ต่อมาเราเจาะลึกที่ระดับจังหวัด โดยสร้าง **heatmap ปริมาณฝนเฉลี่ยรายเดือน** ต่อจังหวัด และจัดอันดับ **Top 5 จังหวัดที่ฝนรวมสูงสุดในแต่ละภาค** 
สิ่งนี้ทำให้เห็นภาพความเสี่ยงที่แตกต่างกันในแต่ละพื้นที่   
 

**Insight:**  
จังหวัดต้นน้ำเช่นเชียงใหม่ เชียงราย และอุบลราชธานีมักเผชิญฝนหนักอย่างต่อเนื่อง ทำให้เสี่ยงเป็นตัวจุดชนวนให้เกิด downstream floods 
ส่วนจังหวัดลุ่มเจ้าพระยา เช่น นครสวรรค์ และอยุธยา อาจไม่ได้มีฝนสูงสุด แต่ได้รับผลสะสมจาก upstream
 
**Central**
Top 5 Provinces with Highest Rainfall in Central: Nakhon Nayok, Bangkok, Samut Prakan, Samut Sakhon, Nonthaburi
<img width="1111" height="479" alt="image" src="https://github.com/user-attachments/assets/e0541d1d-8a22-44ce-9d0e-040d661bc2b3" />

**East**
Top 5 Provinces with Highest Rainfall in East: Trat, Chanthaburi, Prachin Buri, Rayong, Chachoengsao
<img width="1094" height="294" alt="image" src="https://github.com/user-attachments/assets/30a702e2-5fc6-4f42-8551-2496b273741d" />

**Northeast**
Top 5 Provinces with Highest Rainfall in Northeast: Bueng Kan, Nakhon Phanom, Nong Khai, Ubon Ratchathani, Sakon Nakhon 	
<img width="1107" height="691" alt="image" src="https://github.com/user-attachments/assets/b2b6ab3d-2012-4d55-aae9-996b092d5c73" />

**North**
Top 5 Provinces with Highest Rainfall in North: Chiang Rai, Nan, Phayao, Uthai Thani, Tak
<img width="1108" height="586" alt="image" src="https://github.com/user-attachments/assets/d4a66350-aee9-4b9d-b442-74b136dc3ada" />

**South**
Top 5 Provinces with Highest Rainfall in South: Ranong, Phangnga, Narathiwat, Yala, Phuket
<img width="1108" height="486" alt="image" src="https://github.com/user-attachments/assets/7c287946-892d-4863-a9f5-b9ffe93959f5" />

**West**
Top 5 Provinces with Highest Rainfall in West: Prachuap Khiri Khan, Kanchanaburi, Ratchaburi, Phetchaburi, Suphan Buri
<img width="1098" height="291" alt="image" src="https://github.com/user-attachments/assets/d5b79668-e1f1-4398-a9f2-53d867fe20c5" />


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
<img width="1442" height="1038" alt="image" src="https://github.com/user-attachments/assets/48056eec-e1e8-4161-b333-5eb961c6a780" />



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


