<!DOCTYPE html>
<html lang="ar" dir="rtl"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>نداء الفجر — التسجيل</title>
<style>
*{box-sizing:border-box}body{margin:0;font-family:"Noto Naskh Arabic","Segoe UI",Tahoma,sans-serif;background:#F6F4EC;color:#1d2a26}
.top{background:linear-gradient(160deg,#1FA07F,#0F6B53);color:#fff;padding:28px 20px 40px;text-align:center}
.top h1{margin:6px 0 4px;font-size:30px;color:#FFF3C4}.top p{margin:0;opacity:.9}
.card{background:#fff;max-width:520px;margin:-24px auto 24px;border-radius:22px;padding:22px;box-shadow:0 6px 24px rgba(0,0,0,.08)}
label{display:block;font-weight:bold;margin:16px 0 6px;color:#0F6B53}
input,select{width:100%;font:inherit;font-size:18px;padding:12px 14px;border:1.5px solid #d6d2c4;border-radius:14px;background:#fff}
.row{display:flex;gap:8px}.row>*{flex:1}
.seg{display:flex;border:1.5px solid #d6d2c4;border-radius:14px;overflow:hidden}.seg button{flex:1;border:0;background:#fff;font:inherit;padding:12px;font-size:16px;cursor:pointer}
.seg button.on{background:#0F6B53;color:#fff}
.step{display:flex;align-items:center;justify-content:center;gap:14px;direction:ltr}.step button{width:56px;height:56px;border-radius:50%;border:1.5px solid #0F6B53;background:#fff;font-size:26px;color:#0F6B53}
.step b{font-size:34px;min-width:80px;text-align:center;color:#0F6B53}
.go{margin-top:22px;width:100%;padding:16px;font:inherit;font-size:20px;font-weight:bold;border:0;border-radius:16px;background:#D4A93C;color:#0E1A36;cursor:pointer}
.msg{margin-top:14px;text-align:center;font-size:17px}.ok{color:#0F6B53}.bad{color:#B3261E}.muted{color:#6b7a75;font-size:14px;text-align:center}
</style></head><body>
<div class="top"><div style="font-size:42px">🌙</div><h1>نداء الفجر</h1><p id="org">نوقظك لصلاة الفجر باتصال هاتفي</p></div>
<div class="card" id="form">
<label>أنا</label><div class="seg" id="gender"><button data-g="m" class="on">👨 رجل</button><button data-g="f">👩 امرأة</button></div>
<label>الاسم</label><input id="name" placeholder="مثال: أحمد محمد" autocomplete="name">
<label>رقم الهاتف الذي نتصل به</label><input id="phone" type="tel" inputmode="tel" placeholder="مثال: 0933123456" dir="ltr">
<label>مدينتك</label><div class="row"><select id="country"></select><select id="city"></select></div>
<label>أوقظني لـ</label><div class="seg" id="kind"><button data-k="1" class="on">الفجر فقط</button><button data-k="2">التهجد فقط</button><button data-k="3">كلاهما</button></div>
<div id="leadBox"><label>قبل أذان الفجر بـ (دقيقة)</label><div class="step"><button id="minus">−</button><b id="lead">30</b><button id="plus">+</button></div></div>
<button class="go" id="go">سجّلني</button>
<div class="msg" id="msg"></div>
<p class="muted">إذا رنّ هاتفك قبل الفجر فارفض المكالمة، وهكذا نعرف أنك استيقظت دون أي تكلفة.</p>
</div>
<p class="muted">إعداد: مؤسسة إعداد الدعاة</p>
<script>
const D={"countries": [["963", "سوريا"], ["20", "مصر"], ["90", "تركيا"], ["1", "أمريكا وكندا"], ["962", "الأردن"], ["961", "لبنان"], ["966", "السعودية"], ["964", "العراق"], ["970", "فلسطين"], ["965", "الكويت"], ["974", "قطر"], ["971", "الإمارات"], ["973", "البحرين"], ["968", "عُمان"], ["212", "المغرب"], ["213", "الجزائر"], ["216", "تونس"], ["218", "ليبيا"], ["249", "السودان"], ["967", "اليمن"], ["49", "ألمانيا"], ["46", "السويد"], ["31", "هولندا"], ["43", "النمسا"], ["33", "فرنسا"], ["44", "بريطانيا"], ["32", "بلجيكا"]], "cities": [["حمص", "963"], ["دمشق", "963"], ["ريف دمشق", "963"], ["حلب", "963"], ["حماة", "963"], ["اللاذقية", "963"], ["طرطوس", "963"], ["إدلب", "963"], ["دير الزور", "963"], ["الرقة", "963"], ["الحسكة", "963"], ["القامشلي", "963"], ["درعا", "963"], ["السويداء", "963"], ["القنيطرة", "963"], ["منبج", "963"], ["البوكمال", "963"], ["جبلة", "963"], ["بانياس", "963"], ["تدمر", "963"], ["إسطنبول", "90"], ["أنقرة", "90"], ["غازي عنتاب", "90"], ["أورفا", "90"], ["أنطاكيا", "90"], ["مرسين", "90"], ["أضنة", "90"], ["كلس", "90"], ["بورصة", "90"], ["إزمير", "90"], ["قونيا", "90"], ["قيصري", "90"], ["عمان", "962"], ["إربد", "962"], ["الزرقاء", "962"], ["بيروت", "961"], ["طرابلس", "961"], ["صيدا", "961"], ["زحلة", "961"], ["العقبة", "962"], ["المفرق", "962"], ["القاهرة", "20"], ["الجيزة", "20"], ["الإسكندرية", "20"], ["المنصورة", "20"], ["طنطا", "20"], ["الزقازيق", "20"], ["بورسعيد", "20"], ["السويس", "20"], ["الإسماعيلية", "20"], ["دمياط", "20"], ["الفيوم", "20"], ["المنيا", "20"], ["أسيوط", "20"], ["سوهاج", "20"], ["قنا", "20"], ["الأقصر", "20"], ["أسوان", "20"], ["الغردقة", "20"], ["مكة المكرمة", "966"], ["المدينة المنورة", "966"], ["الرياض", "966"], ["جدة", "966"], ["الدمام", "966"], ["أبها", "966"], ["بغداد", "964"], ["الموصل", "964"], ["البصرة", "964"], ["أربيل", "964"], ["الأنبار (الرمادي)", "964"], ["القدس", "970"], ["غزة", "970"], ["الخليل", "970"], ["نابلس", "970"], ["الكويت", "965"], ["الدوحة", "974"], ["دبي", "971"], ["أبوظبي", "971"], ["الشارقة", "971"], ["المنامة", "973"], ["مسقط", "968"], ["الدار البيضاء", "212"], ["الرباط", "212"], ["الجزائر", "213"], ["تونس", "216"], ["طرابلس الغرب", "218"], ["الخرطوم", "249"], ["صنعاء", "967"], ["عدن", "967"], ["برلين", "49"], ["هامبورغ", "49"], ["ميونخ", "49"], ["كولونيا", "49"], ["فرانكفورت", "49"], ["ستوكهولم", "46"], ["أمستردام", "31"], ["فيينا", "43"], ["باريس", "33"], ["لندن", "44"], ["بروكسل", "32"], ["نيويورك", "1"], ["شيكاغو", "1"], ["هيوستن", "1"], ["لوس أنجلوس", "1"], ["ديترويت", "1"], ["ديربورن", "1"], ["دالاس", "1"], ["واشنطن", "1"], ["فيلادلفيا", "1"], ["سان فرانسيسكو", "1"], ["ميامي", "1"], ["أتلانتا", "1"], ["فينيكس", "1"], ["باترسون", "1"], ["تورونتو (كندا)", "1"], ["مونتريال (كندا)", "1"]]};
let cfg={};try{let h=location.hash.slice(1).replace(/-/g,'+').replace(/_/g,'/');while(h.length%4)h+='=';cfg=JSON.parse(decodeURIComponent(escape(atob(h))))}catch(e){}
const $=id=>document.getElementById(id);
if(cfg.o)$('org').textContent='من '+cfg.o+' — نوقظك لصلاة الفجر باتصال هاتفي';
const defCity=cfg.city||'حمص';const defCC=(D.cities.find(c=>c[0]==defCity)||['','963'])[1];
D.countries.forEach(([cc,n])=>{const o=new Option(n,cc);$('country').add(o)});$('country').value=defCC;
function fill(){const cc=$('country').value;$('city').innerHTML='';D.cities.filter(c=>c[1]==cc).forEach(c=>$('city').add(new Option(c[0],c[0])));if(cc==defCC)$('city').value=defCity}
$('country').onchange=fill;fill();
let kind=1,lead=30,gender='m';document.querySelectorAll('#gender button').forEach(b=>b.onclick=e=>{e.preventDefault();document.querySelectorAll('#gender button').forEach(x=>x.classList.remove('on'));b.classList.add('on');gender=b.dataset.g});const smsMode=!(cfg.s&&cfg.k&&cfg.c);if(smsMode)$('leadBox').style.display='none';
document.querySelectorAll('#kind button').forEach(b=>b.onclick=e=>{e.preventDefault();document.querySelectorAll('#kind button').forEach(x=>x.classList.remove('on'));b.classList.add('on');kind=+b.dataset.k;$('leadBox').style.display=(kind==2||smsMode)?'none':''});
$('minus').onclick=e=>{e.preventDefault();lead=Math.max(1,lead-1);$('lead').textContent=lead};
$('plus').onclick=e=>{e.preventDefault();lead=Math.min(180,lead+1);$('lead').textContent=lead};
function say(t,ok){$('msg').className='msg '+(ok?'ok':'bad');$('msg').textContent=t}
$('go').onclick=async e=>{e.preventDefault();
 const name=$('name').value.trim(),phone=$('phone').value.replace(/[^\d+]/g,''),city=$('city').value;
 if(!name)return say('اكتب اسمك');if(phone.replace(/\D/g,'').length<7)return say('اكتب رقم هاتف صحيحاً');
 const body={name,phone,city,gender,fajr:kind!=2?1:0,tah:kind!=1?1:0,lead};
 if(cfg.s&&cfg.k&&cfg.c){
  say('جارٍ التسجيل…',true);$('go').disabled=true;
  const h={'apikey':cfg.k,'Content-Type':'application/json'};if(cfg.k.startsWith('eyJ'))h.Authorization='Bearer '+cfg.k;
  try{const r=await fetch(cfg.s.replace(/\/$/,'')+'/rest/v1/rpc/nidaa_api',{method:'POST',headers:h,body:JSON.stringify({p_method:'POST',p_path:'/api/signup',p_body:body,p_code:cfg.c,p_admin:'',p_device:''})});
   const j=await r.json();
   if(j&&j.ok){$('form').innerHTML='<div style="text-align:center;font-size:54px">✅</div><h2 style="text-align:center;color:#0F6B53">'+(j.exists?'أنت مسجّل من قبل':'تم تسجيلك')+' يا '+(j.name||name)+'</h2><p class="muted" style="font-size:16px">سنتصل بك قبل الفجر حسب وقت '+city+'. إذا رنّ هاتفك فارفض المكالمة.</p>'}
   else{say((j&&(j.error||j.message))||'تعذّر التسجيل، حاول مرة أخرى');$('go').disabled=false}
  }catch(x){say('تعذّر الاتصال بالإنترنت، حاول مرة أخرى');$('go').disabled=false}
 } else if(cfg.p){
  const k=kind==1?'فجر':kind==2?'تهجد':'كلاهما';const txt='اشتراك '+k+' '+(gender=='f'?'أخت ':'')+name+' '+city;
  const sep=/iPhone|iPad/.test(navigator.userAgent)?'&':'?';
  location.href='sms:'+cfg.p+sep+'body='+encodeURIComponent(txt);
  say('اضغط «إرسال» في الرسالة التي فُتحت، وسيصلك ردّ بالتسجيل.',true);
 } else say('الرابط ناقص، اطلب رابطاً جديداً');
};
</script></body></html>
