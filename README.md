# Մասնագետ — հայտերի մարքեթփլեյս (Next.js + Supabase + Idram)

## Գործարկում
1. `npm install`
2. Supabase-ում ստեղծիր նախագիծ → SQL Editor-ում աշխատեցրու `supabase/schema.sql`
3. `.env.example`-ը պատճենիր `.env.local` և լրացրու (Supabase Settings → API)
4. Supabase → Authentication → URL Configuration․ Site URL = քո դոմենը, Redirect URL = `<դոմեն>/auth/callback`
5. `npm run dev`

## Idram
- Ստորագրիր merchant պայմանագիր Idram-ի հետ, ստացիր `EDP_REC_ACCOUNT` և `SECRET_KEY`
- Idram-ին տուր՝ RESULT_URL `https://<դոմեն>/api/payments/idram/callback`, SUCCESS_URL `https://<դոմեն>/payment/success`, FAIL_URL `https://<դոմեն>/payment/fail`
- `app/api/payments/idram/callback/route.ts`-ում checksum-ի բանաձևը համեմատիր Idram-ի փաստաթղթի հետ

## Անվտանգություն
- Հեռախոսները `lead_contacts`-ում են, RLS-ը տալիս է միայն գնորդին
- Գնումը և լիցքավորումը ատոմար SQL ֆունկցիաներ են (`purchase_lead`, `credit_payment`), կանչվում են միայն service role-ով
- Balance/role-ը կլիենտից փոխել հնարավոր չէ (column-level grant)
- Հայտի ձևում՝ honeypot։ Production-ում ավելացրու Cloudflare Turnstile + rate limit
