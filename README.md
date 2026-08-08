# תוכנית הקיץ

מתכנן יומי לפי שעות, 8 באוגוסט – 1 בספטמבר 2026. עיצוב בהשראת Adobe Lightroom Classic.
קובץ אחד, בלי build, בלי dependencies.

## סנכרון בין מכשירים

כברירת מחדל הנתונים נשמרים ב-localStorage של הדפדפן בלבד.
כדי לסנכרן בין המחשב לפלאפון:

1. ב-[supabase.com](https://supabase.com) → פרויקט (חדש או קיים) → **SQL Editor** → הרץ:

   ```sql
   create table plan (id int primary key, doc jsonb, updated_at timestamptz default now());
   alter table plan enable row level security;
   create policy "open" on plan for all using (true) with check (true);
   ```

2. **Project Settings → API** → העתק את `Project URL` ואת מפתח ה-`anon public`.

3. ב-[index.html](index.html), בשורת `CONFIG` בראש ה-`<script>`:

   ```js
   const CONFIG = { url:"https://xxxx.supabase.co", key:"eyJhbGci..." };
   ```

4. commit + push. GitHub Pages מתעדכן תוך כדקה.

**שים לב:** ה-policy פתוחה, ומפתח ה-anon גלוי בקוד של אתר ציבורי — כלומר מי שמגיע ל-URL של הפרויקט יכול לקרוא ולכתוב. מספיק ללוח פעילויות משפחתי, לא למידע רגיש.

## איך זה עובד

- **טווח שעות** (פאנל ימני) קובע אילו שעות מוצגות בכל יום. הוא רק תבנית תצוגה — הוא לא מוחק כלום.
- כל שעה ניתנת לעריכה בלחיצה עליה (אפשר להקליד `07:30`), ו-**+ הוסף שעה** מוסיף שורה משלך.
- טקסט נשמר לפי שעה, כך ששינוי הטווח לא מאבד מה שכבר נכתב.
- **ייצוא לקובץ** = גיבוי JSON מלא. **ייבוא** מחזיר אותו.
