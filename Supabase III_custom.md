# sec_supabase
في Table Editor يوجد New Table اسمه users هو Row مكون من عدة Columns:
- id (uuid)
- email (text)
- username (text)
- image (text)
- created_at (timestamptz)
- password (text)

ثم Create police في ال Auth Policies:
  (auth.uid() = id)

  
![police](https://github.com/user-attachments/assets/f5cd5514-7e1d-47db-a98b-c8942b000614)


ثم في Providers أجعل ال Email => Enabled.

وفي ال Storage انشيء New Bucket باسم user-photos واجعله Public
وفي ال Policies:


![police](https://github.com/user-attachments/assets/3ef05ace-11df-40f1-8cf2-c2fd40f778ac)
