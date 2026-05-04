import { useState } from "react";

const LOGO = "/logo.jpg";
const OYUNGEREL_PHOTO = "/oyungerel.jpg";
const NARANTSETSEG_PHOTO = "/narantsetseg.jpg";
const CLINIC_PHONE = "99001234"; // ← Эмнэлгийн дугаарыг энд өөрчилнө

const DOCTORS = [
  {
    id: 1,
    name: "Д-р Б.Оюунгэрэл",
    specialty: "Эмэгтэйчүүдийн эмч",
    img: OYUNGEREL_PHOTO,
    rating: 4.9, reviews: 156, exp: "10 жил",
    price: 45000, priceLabel: "45,000₮",
    whatsapp: "97612345", // ← Эмчийн WhatsApp дугаар
    viber: "97612345",    // ← Эмчийн Viber дугаар
    tags: ["Жирэмслэлт", "Эмэгтэйчүүд", "Үр шилжүүлэг"],
    available: ["09:00", "10:00", "11:00", "14:00", "15:00", "16:00"],
    bio: "Эмэгтэйчүүдийн өвчин, жирэмслэлт, төрөлт болон нөхөн үржихүйн эрүүл мэндэд мэргэшсэн. Олон улсын хурал, сургалтад идэвхтэй оролцдог.",
    education: "АШУҮИС, Эмэгтэйчүүдийн эмч мэргэжил",
  },
  {
    id: 2,
    name: "Д-р Г.Наранцэцэг",
    specialty: "Эмэгтэйчүүдийн эмч",
    img: NARANTSETSEG_PHOTO,
    rating: 4.9, reviews: 98, exp: "10 жил",
    price: 30000, priceLabel: "30,000₮",
    whatsapp: "97698765", // ← Эмчийн WhatsApp дугаар
    viber: "97698765",    // ← Эмчийн Viber дугаар
    tags: ["Жирэмслэлт", "Эмэгтэйчүүд", "Нөхөн үржихүй"],
    available: ["09:00", "10:00", "11:00", "14:00", "15:00", "16:00"],
    bio: "Эмэгтэйчүүдийн нөхөн үржихүйн эрүүл мэнд, жирэмслэлтийн хяналт болон эмэгтэйчүүдийн өвчин эмгэгт мэргэшсэн эмч.",
    education: "АШУҮИС, Эмэгтэйчүүдийн эмч мэргэжил",
  },
];

const HEALTH_TOPICS = [
  { id: 1, icon: "👩", title: "Эмэгтэйчүүдийн эрүүл мэнд", color: "#ec4899", summary: "Жирэмслэлт, нөхөн үржихүйн эрүүл мэнд болон эмэгтэйчүүдэд онцлог өвчнүүд.", sections: [{ title: "Шинж тэмдэг", content: "Тогтмол бус сарын тэмдэг, доод хэвлийн өвдөлт, цагаан ялгадас, жирэмсний шинж тэмдгүүд." }, { title: "Урьдчилан сэргийлэх", content: "Жил бүр эмэгтэйчүүдийн эмчид үзүүлэх, Паап шинжилгээ өгөх, эрт илрүүлэх нь чухал." }] },
  { id: 2, icon: "🤰", title: "Жирэмслэлтийн хяналт", color: "#f97316", summary: "Жирэмслэлтийн үеийн эрүүл мэндийн хяналт, зөвлөгөө.", sections: [{ title: "Шинж тэмдэг", content: "Дотор муухайрах, ядаргаа, хөх өвдөх, сарын тэмдэг зогсох зэрэг эрт шинж тэмдгүүд." }, { title: "Урьдчилан сэргийлэх", content: "Тогтмол эмчид үзүүлэх, фолийн хүчил хэрэглэх, эрүүл хоол хүнс идэх." }] },
  { id: 3, icon: "🩸", title: "Сарын тэмдгийн асуудал", color: "#ef4444", summary: "Тогтмол бус, өвдөлттэй сарын тэмдэг болон холбогдох өвчнүүд.", sections: [{ title: "Шинж тэмдэг", content: "Хэт их цус алдах, хүчтэй өвдөлт, тогтмол бус давтамж, урт хугацааны үргэлжлэл." }, { title: "Урьдчилан сэргийлэх", content: "Тогтмол дасгал хийх, стрессээс зайлсхийх, эмчид цаг тухайд нь үзүүлэх." }] },
  { id: 4, icon: "💊", title: "Дотоод шүүрлийн өвчин", color: "#8b5cf6", summary: "Гормоны тэнцвэргүй байдал болон дотоод шүүрлийн өвчнүүд.", sections: [{ title: "Шинж тэмдэг", content: "Жин өөрчлөгдөх, сэтгэл санааны хэлбэлзэл, ядаргаа, арьсны өөрчлөлт." }, { title: "Урьдчилан сэргийлэх", content: "Тогтмол шинжилгээ өгөх, эрүүл хоол хүнс, дасгал хөдөлгөөн чухал." }] },
];

const C = {
  bg: "#fdf2f8", white: "#ffffff", primary: "#0057ff", primaryLight: "#e8f0ff",
  text: "#0f1d35", muted: "#6b7c99", border: "#f9a8d4",
  success: "#00c48c", pink: "#ec4899", pinkLight: "#fdf2f8",
  blue: "#3b5998", purple: "#7b519c",
};

function StarRating({ rating }) {
  return <span style={{ color: "#f59e0b", fontSize: 13 }}>{"★".repeat(Math.floor(rating))}{"☆".repeat(5 - Math.floor(rating))} {rating}</span>;
}

// ─── BOTTOM NAV ──────────────────────────────────────────────────────────────
function BottomNav({ tab, setTab }) {
  const tabs = [
    { id: "home", icon: "🏠", label: "Нүүр" },
    { id: "doctors", icon: "👩‍⚕️", label: "Эмч нар" },
    { id: "health", icon: "📋", label: "Мэдээлэл" },
    { id: "appointments", icon: "📅", label: "Цаг" },
    { id: "profile", icon: "👤", label: "Профайл" },
  ];
  return (
    <div style={{ position: "fixed", bottom: 0, left: "50%", transform: "translateX(-50%)", width: "100%", maxWidth: 430, background: C.white, borderTop: `1px solid ${C.border}`, display: "flex", zIndex: 100, boxShadow: "0 -4px 20px rgba(236,72,153,0.1)" }}>
      {tabs.map(t => (
        <button key={t.id} onClick={() => setTab(t.id)} style={{ flex: 1, padding: "10px 0 14px", border: "none", background: "none", cursor: "pointer", display: "flex", flexDirection: "column", alignItems: "center", gap: 3, color: tab === t.id ? C.pink : C.muted }}>
          <span style={{ fontSize: 20 }}>{t.icon}</span>
          <span style={{ fontSize: 10, fontWeight: tab === t.id ? 700 : 400 }}>{t.label}</span>
          {tab === t.id && <div style={{ width: 20, height: 3, background: C.pink, borderRadius: 99, marginTop: -4 }} />}
        </button>
      ))}
    </div>
  );
}

// ─── CALENDAR ────────────────────────────────────────────────────────────────
function Calendar({ onSelect, selected }) {
  const today = new Date();
  const [viewMonth, setViewMonth] = useState(today.getMonth());
  const [viewYear, setViewYear] = useState(today.getFullYear());
  const MONTHS = ["1-р сар","2-р сар","3-р сар","4-р сар","5-р сар","6-р сар","7-р сар","8-р сар","9-р сар","10-р сар","11-р сар","12-р сар"];
  const DAYS = ["Да","Мя","Лх","Пү","Ба","Бя","Ня"];
  const firstDay = new Date(viewYear, viewMonth, 1).getDay();
  const daysInMonth = new Date(viewYear, viewMonth + 1, 0).getDate();
  const startOffset = firstDay === 0 ? 6 : firstDay - 1;
  const prevMonth = () => { if (viewMonth === 0) { setViewMonth(11); setViewYear(y=>y-1); } else setViewMonth(m=>m-1); };
  const nextMonth = () => { if (viewMonth === 11) { setViewMonth(0); setViewYear(y=>y+1); } else setViewMonth(m=>m+1); };
  const isWeekend = d => { const day = new Date(viewYear,viewMonth,d).getDay(); return day===0||day===6; };
  const isPast = d => { const dt=new Date(viewYear,viewMonth,d); dt.setHours(0,0,0,0); const t=new Date(); t.setHours(0,0,0,0); return dt<t; };
  const isSelected = d => selected && selected.getDate()===d && selected.getMonth()===viewMonth && selected.getFullYear()===viewYear;
  const isToday = d => today.getDate()===d && today.getMonth()===viewMonth && today.getFullYear()===viewYear;
  const cells = [];
  for (let i=0;i<startOffset;i++) cells.push(null);
  for (let i=1;i<=daysInMonth;i++) cells.push(i);
  return (
    <div style={{ background: C.white, border:`1px solid ${C.border}`, borderRadius:16, padding:16 }}>
      <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:12 }}>
        <button onClick={prevMonth} style={{ background:C.pinkLight, border:"none", borderRadius:8, width:32, height:32, cursor:"pointer", fontSize:16, color:C.pink }}>‹</button>
        <span style={{ fontWeight:700, fontSize:15, color:C.text }}>{viewYear} · {MONTHS[viewMonth]}</span>
        <button onClick={nextMonth} style={{ background:C.pinkLight, border:"none", borderRadius:8, width:32, height:32, cursor:"pointer", fontSize:16, color:C.pink }}>›</button>
      </div>
      <div style={{ display:"grid", gridTemplateColumns:"repeat(7,1fr)", gap:4, marginBottom:8 }}>
        {DAYS.map(d=><div key={d} style={{ textAlign:"center", fontSize:11, fontWeight:700, color:C.muted, padding:"4px 0" }}>{d}</div>)}
      </div>
      <div style={{ display:"grid", gridTemplateColumns:"repeat(7,1fr)", gap:4 }}>
        {cells.map((day,i)=>{
          if(!day) return <div key={i}/>;
          const disabled=isWeekend(day)||isPast(day);
          const sel=isSelected(day); const tod=isToday(day);
          return <button key={i} onClick={()=>!disabled&&onSelect(new Date(viewYear,viewMonth,day))} style={{ width:"100%", aspectRatio:"1", borderRadius:8, border:"none", cursor:disabled?"not-allowed":"pointer", background:sel?C.pink:tod?C.pinkLight:"none", color:disabled?"#ddd":sel?"#fff":isWeekend(day)?"#fca5a5":C.text, fontWeight:sel||tod?700:400, fontSize:13 }}>{day}</button>;
        })}
      </div>
      <div style={{ display:"flex", gap:12, marginTop:10, fontSize:11, color:C.muted }}>
        <span>🔴 Амралтын өдөр</span><span style={{color:C.pink}}>● Сонгогдсон</span>
      </div>
    </div>
  );
}

// ─── CONTACT MODAL (WhatsApp / Viber) ────────────────────────────────────────
function ContactModal({ doc, appointment, onClose }) {
  const dateStr = appointment?.date
    ? `${appointment.date.getFullYear()}-${appointment.date.getMonth()+1}-${appointment.date.getDate()}`
    : "";
  const message = encodeURIComponent(`Сайн байна уу, би Төгсмед эмнэлгээр дамжуулан цаг захиалсан.\nЭмч: ${doc.name}\nОгноо: ${dateStr}\nЦаг: ${appointment?.time}\nТаны зөвлөгөөг авахаар холбогдож байна.`);

  return (
    <div style={{ position:"fixed", inset:0, background:"rgba(0,0,0,0.6)", zIndex:300, display:"flex", alignItems:"flex-end", justifyContent:"center" }}>
      <div style={{ background:C.white, borderRadius:"24px 24px 0 0", width:"100%", maxWidth:430, padding:"28px 20px 44px" }}>
        <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:20 }}>
          <div style={{ fontWeight:800, fontSize:18, color:C.text }}>📞 Эмчтэй холбогдох</div>
          <button onClick={onClose} style={{ background:"#f1f5f9", border:"none", borderRadius:50, width:32, height:32, cursor:"pointer", fontSize:16 }}>✕</button>
        </div>

        {/* Doctor info */}
        <div style={{ background:C.pinkLight, borderRadius:16, padding:16, marginBottom:20, display:"flex", gap:14, alignItems:"center" }}>
          <img src={doc.img} style={{ width:56, height:56, borderRadius:50, objectFit:"cover", objectPosition:"center top", border:`2px solid ${C.border}` }} />
          <div>
            <div style={{ fontWeight:700, fontSize:15, color:C.text }}>{doc.name}</div>
            <div style={{ fontSize:12, color:C.pink }}>{doc.specialty}</div>
            <div style={{ fontSize:12, color:C.muted, marginTop:2 }}>{dateStr} · {appointment?.time}</div>
          </div>
        </div>

        <div style={{ fontSize:13, color:C.muted, marginBottom:16, lineHeight:1.6 }}>
          💡 Төлбөр амжилттай төлөгдсөн. Доорх сувгуудаар эмчтэйгээ шууд холбогдоно уу. Жор болон зургийг мессежээр хүлээн авах боломжтой.
        </div>

        {/* WhatsApp */}
        <a href={`https://wa.me/976${doc.whatsapp}?text=${message}`} target="_blank" rel="noopener noreferrer"
          style={{ display:"flex", alignItems:"center", gap:14, background:"#25D366", borderRadius:16, padding:"16px 20px", marginBottom:12, textDecoration:"none" }}>
          <div style={{ width:44, height:44, borderRadius:12, background:"rgba(255,255,255,0.2)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:24 }}>💬</div>
          <div>
            <div style={{ fontWeight:700, fontSize:15, color:"#fff" }}>WhatsApp-аар холбогдох</div>
            <div style={{ fontSize:12, color:"rgba(255,255,255,0.85)" }}>+976 {doc.whatsapp} · Мессеж, зураг, жор</div>
          </div>
          <span style={{ marginLeft:"auto", color:"rgba(255,255,255,0.8)", fontSize:20 }}>→</span>
        </a>

        {/* Viber */}
        <a href={`viber://chat?number=976${doc.viber}`} target="_blank" rel="noopener noreferrer"
          style={{ display:"flex", alignItems:"center", gap:14, background:"#7b519c", borderRadius:16, padding:"16px 20px", marginBottom:12, textDecoration:"none" }}>
          <div style={{ width:44, height:44, borderRadius:12, background:"rgba(255,255,255,0.2)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:24 }}>📱</div>
          <div>
            <div style={{ fontWeight:700, fontSize:15, color:"#fff" }}>Viber-ээр холбогдох</div>
            <div style={{ fontSize:12, color:"rgba(255,255,255,0.85)" }}>+976 {doc.viber} · Дуудлага, мессеж</div>
          </div>
          <span style={{ marginLeft:"auto", color:"rgba(255,255,255,0.8)", fontSize:20 }}>→</span>
        </a>

        {/* Phone call */}
        <a href={`tel:+976${doc.whatsapp}`}
          style={{ display:"flex", alignItems:"center", gap:14, background:C.primaryLight, borderRadius:16, padding:"16px 20px", textDecoration:"none" }}>
          <div style={{ width:44, height:44, borderRadius:12, background:"rgba(0,87,255,0.1)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:24 }}>📞</div>
          <div>
            <div style={{ fontWeight:700, fontSize:15, color:C.primary }}>Шууд залгах</div>
            <div style={{ fontSize:12, color:C.muted }}>+976 {doc.whatsapp}</div>
          </div>
          <span style={{ marginLeft:"auto", color:C.primary, fontSize:20 }}>→</span>
        </a>
      </div>
    </div>
  );
}

// ─── QPAY MODAL ──────────────────────────────────────────────────────────────
function QPayModal({ appointment, onClose, onSuccess }) {
  const [step, setStep] = useState("choose");
  const [paying, setPaying] = useState(false);
  const banks = [
    { name:"Хаан банк", color:"#00a651" }, { name:"Голомт банк", color:"#e31e24" },
    { name:"Төрийн банк", color:"#003087" }, { name:"Хас банк", color:"#f7941d" },
    { name:"Монгол банк", color:"#1a237e" }, { name:"Капитрон банк", color:"#006837" },
  ];
  const dateStr = appointment?.date ? `${appointment.date.getFullYear()}-${appointment.date.getMonth()+1}-${appointment.date.getDate()}` : "";
  const simulatePay = () => { setPaying(true); setTimeout(()=>{ setPaying(false); setStep("done"); }, 2500); };

  return (
    <div style={{ position:"fixed", inset:0, background:"rgba(0,0,0,0.6)", zIndex:300, display:"flex", alignItems:"flex-end", justifyContent:"center" }}>
      <div style={{ background:C.white, borderRadius:"24px 24px 0 0", width:"100%", maxWidth:430, maxHeight:"90vh", overflowY:"auto", padding:"24px 20px 40px" }}>
        <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:20 }}>
          <div style={{ fontWeight:800, fontSize:18, color:C.text }}>{step==="done"?"✅ Амжилттай!":"💳 QPay төлбөр"}</div>
          {step!=="done" && <button onClick={onClose} style={{ background:"#f1f5f9", border:"none", borderRadius:50, width:32, height:32, cursor:"pointer" }}>✕</button>}
        </div>
        <div style={{ background:"linear-gradient(135deg, #ec4899, #f472b6)", borderRadius:18, padding:"18px 20px", marginBottom:20, color:"#fff" }}>
          <div style={{ fontSize:12, opacity:0.8, marginBottom:4 }}>Төлөх дүн</div>
          <div style={{ fontSize:32, fontWeight:800 }}>{appointment?.doc?.priceLabel}</div>
          <div style={{ fontSize:13, opacity:0.8, marginTop:6 }}>{appointment?.doc?.name} · {dateStr} · {appointment?.time}</div>
        </div>

        {step==="choose" && (
          <>
            <div style={{ fontWeight:700, color:C.text, marginBottom:14 }}>Банкаа сонгоно уу</div>
            <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:10, marginBottom:20 }}>
              {banks.map((b,i)=>(
                <button key={i} onClick={()=>setStep("qr")} style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:14, padding:"14px 10px", cursor:"pointer", display:"flex", alignItems:"center", gap:10 }}>
                  <div style={{ width:32, height:32, borderRadius:8, background:b.color }}/>
                  <span style={{ fontSize:12, fontWeight:600, color:C.text }}>{b.name}</span>
                </button>
              ))}
            </div>
          </>
        )}

        {step==="qr" && (
          <div style={{ textAlign:"center" }}>
            <div style={{ fontSize:13, color:C.muted, marginBottom:16 }}>QPay QR кодыг уншуулж төлнө үү</div>
            <div style={{ width:180, height:180, margin:"0 auto 20px", background:C.white, border:`2px solid ${C.border}`, borderRadius:16, display:"flex", alignItems:"center", justifyContent:"center" }}>
              <svg width="160" height="160" viewBox="0 0 180 180">
                {[...Array(9)].map((_,r)=>[...Array(9)].map((_,c)=>{
                  const isCorner=(r<3&&c<3)||(r<3&&c>5)||(r>5&&c<3);
                  const rand=((r*9+c)*37+13)%17>8;
                  return <rect key={`${r}-${c}`} x={10+c*18} y={10+r*18} width={16} height={16} rx={2} fill={isCorner||rand?"#ec4899":"#fdf2f8"}/>;
                }))}
              </svg>
            </div>
            <button onClick={simulatePay} disabled={paying} style={{ width:"100%", padding:16, borderRadius:14, border:"none", background:paying?"#94a3b8":C.pink, color:"#fff", fontWeight:700, fontSize:16, cursor:paying?"wait":"pointer" }}>
              {paying?"⏳ Шалгаж байна...":"✅ Төлбөр хийсэн"}
            </button>
          </div>
        )}

        {step==="done" && (
          <div style={{ padding:"10px 0" }}>
            <div style={{ textAlign:"center", marginBottom:20 }}>
              <div style={{ fontSize:56, marginBottom:8 }}>🎉</div>
              <div style={{ fontSize:20, fontWeight:800, color:C.text, marginBottom:6 }}>Амжилттай төлөгдлөо!</div>
              <div style={{ fontSize:14, color:C.muted, lineHeight:1.6 }}>
                {appointment?.doc?.name}-д цаг захиалсан.<br/>{dateStr} · {appointment?.time}
              </div>
            </div>

            {/* Doctor contact - only shown after payment */}
            <div style={{ background:"#f0fdf4", border:"1px solid #86efac", borderRadius:14, padding:14, marginBottom:16 }}>
              <div style={{ fontWeight:700, color:"#16a34a", fontSize:13, marginBottom:4 }}>✅ Төлбөр баталгаажлаа — эмчтэй холбогдоно уу</div>
              <div style={{ fontSize:12, color:"#15803d" }}>Доорх дугаараар мессеж эсвэл дуудлага хийнэ үү</div>
            </div>

            {/* WhatsApp */}
            <a href={`https://wa.me/976${appointment?.doc?.whatsapp}?text=${encodeURIComponent(`Сайн байна уу, би Төгсмед эмнэлгээр дамжуулан цаг захиалсан.\nЭмч: ${appointment?.doc?.name}\nОгноо: ${dateStr}\nЦаг: ${appointment?.time}`)}`}
              target="_blank" rel="noopener noreferrer"
              style={{ display:"flex", alignItems:"center", gap:12, background:"#25D366", borderRadius:14, padding:"14px 16px", marginBottom:10, textDecoration:"none" }}>
              <div style={{ width:40, height:40, borderRadius:10, background:"rgba(255,255,255,0.2)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:22 }}>💬</div>
              <div style={{ flex:1 }}>
                <div style={{ fontWeight:700, fontSize:14, color:"#fff" }}>WhatsApp</div>
                <div style={{ fontSize:12, color:"rgba(255,255,255,0.85)" }}>+976 {appointment?.doc?.whatsapp}</div>
              </div>
              <span style={{ color:"rgba(255,255,255,0.8)", fontSize:18 }}>→</span>
            </a>

            {/* Viber */}
            <a href={`viber://chat?number=976${appointment?.doc?.viber}`}
              target="_blank" rel="noopener noreferrer"
              style={{ display:"flex", alignItems:"center", gap:12, background:"#7b519c", borderRadius:14, padding:"14px 16px", marginBottom:10, textDecoration:"none" }}>
              <div style={{ width:40, height:40, borderRadius:10, background:"rgba(255,255,255,0.2)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:22 }}>📱</div>
              <div style={{ flex:1 }}>
                <div style={{ fontWeight:700, fontSize:14, color:"#fff" }}>Viber</div>
                <div style={{ fontSize:12, color:"rgba(255,255,255,0.85)" }}>+976 {appointment?.doc?.viber}</div>
              </div>
              <span style={{ color:"rgba(255,255,255,0.8)", fontSize:18 }}>→</span>
            </a>

            {/* Phone */}
            <a href={`tel:+976${appointment?.doc?.whatsapp}`}
              style={{ display:"flex", alignItems:"center", gap:12, background:C.primaryLight, borderRadius:14, padding:"14px 16px", marginBottom:20, textDecoration:"none" }}>
              <div style={{ width:40, height:40, borderRadius:10, background:"rgba(0,87,255,0.1)", display:"flex", alignItems:"center", justifyContent:"center", fontSize:22 }}>📞</div>
              <div style={{ flex:1 }}>
                <div style={{ fontWeight:700, fontSize:14, color:C.primary }}>Шууд залгах</div>
                <div style={{ fontSize:12, color:C.muted }}>+976 {appointment?.doc?.whatsapp}</div>
              </div>
              <span style={{ color:C.primary, fontSize:18 }}>→</span>
            </a>

            <button onClick={()=>{ onSuccess(); onClose(); }}
              style={{ width:"100%", padding:14, borderRadius:14, border:`1px solid ${C.border}`, background:"none", color:C.muted, fontSize:14, cursor:"pointer" }}>
              Миний цагуудыг харах
            </button>
          </div>
        )}
      </div>
    </div>
  );
}

// ─── DOCTOR DETAIL ───────────────────────────────────────────────────────────
function DoctorDetail({ doc, onBack, onBook }) {
  const [selTime, setSelTime] = useState(null);
  const [selDate, setSelDate] = useState(null);
  const [showQPay, setShowQPay] = useState(false);
  const [showContact, setShowContact] = useState(false);
  const [pendingAppt, setPendingAppt] = useState(null);
  const [paidAppt, setPaidAppt] = useState(null);

  const handleBook = () => {
    if (!selTime || !selDate) return;
    const appt = { doc, time:selTime, date:selDate };
    setPendingAppt(appt);
    setShowQPay(true);
  };

  const handlePaySuccess = () => {
    const appt = { doc, time:selTime, date:selDate };
    onBook(appt);
    setPaidAppt(appt);
    setShowQPay(false);
    setShowContact(true);
  };

  const dateLabel = selDate ? `${selDate.getFullYear()}-${selDate.getMonth()+1}-${selDate.getDate()}` : null;

  return (
    <div style={{ padding:"20px 16px" }}>
      {showQPay && pendingAppt && <QPayModal appointment={pendingAppt} onClose={()=>setShowQPay(false)} onSuccess={handlePaySuccess}/>}
      {showContact && paidAppt && <ContactModal doc={doc} appointment={paidAppt} onClose={()=>setShowContact(false)}/>}

      <button onClick={onBack} style={{ background:"none", border:"none", fontSize:22, cursor:"pointer", marginBottom:12, color:C.pink }}>←</button>

      <div style={{ background:`linear-gradient(135deg, ${C.pink}, #f472b6)`, borderRadius:24, padding:"24px 20px", textAlign:"center", marginBottom:20 }}>
        <img src={doc.img} style={{ width:100, height:100, borderRadius:50, objectFit:"cover", objectPosition:"center top", border:"3px solid rgba(255,255,255,0.5)", marginBottom:12 }} onError={e=>e.target.style.display="none"}/>
        <div style={{ fontSize:20, fontWeight:800, color:"#fff" }}>{doc.name}</div>
        <div style={{ fontSize:14, color:"rgba(255,255,255,0.85)", marginBottom:4 }}>{doc.specialty}</div>
        <div style={{ fontSize:12, color:"rgba(255,255,255,0.7)", marginBottom:8 }}>{doc.education}</div>
        <StarRating rating={doc.rating}/>
        <div style={{ marginTop:10, display:"inline-block", background:"rgba(255,255,255,0.2)", borderRadius:20, padding:"4px 14px", fontSize:12, color:"#fff" }}>✓ Баталгаажсан эмч</div>
      </div>

      <div style={{ display:"flex", gap:10, marginBottom:16 }}>
        {[{icon:"🏆",label:"Туршлага",val:doc.exp},{icon:"💰",label:"Үнэ",val:doc.priceLabel},{icon:"⭐",label:"Үнэлгээ",val:`${doc.rating}/5`}].map((s,i)=>(
          <div key={i} style={{ flex:1, background:C.pinkLight, borderRadius:14, padding:"12px 8px", textAlign:"center" }}>
            <div style={{ fontSize:20, marginBottom:4 }}>{s.icon}</div>
            <div style={{ fontSize:13, fontWeight:700, color:C.pink }}>{s.val}</div>
            <div style={{ fontSize:10, color:C.muted }}>{s.label}</div>
          </div>
        ))}
      </div>

      <div style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:16, padding:16, marginBottom:16 }}>
        <div style={{ fontWeight:700, marginBottom:8, color:C.text }}>Тухай</div>
        <div style={{ fontSize:14, color:C.muted, lineHeight:1.7 }}>{doc.bio}</div>
      </div>

      <div style={{ fontWeight:700, marginBottom:10, color:C.text }}>📅 Огноо сонгох</div>
      <div style={{ marginBottom:16 }}><Calendar onSelect={setSelDate} selected={selDate}/></div>

      {selDate && (
        <>
          <div style={{ fontWeight:700, marginBottom:10, color:C.text }}>⏰ {dateLabel} — Цаг сонгох</div>
          <div style={{ display:"flex", flexWrap:"wrap", gap:8, marginBottom:20 }}>
            {doc.available.map((t,i)=>(
              <button key={i} onClick={()=>setSelTime(t)} style={{ padding:"10px 16px", borderRadius:12, cursor:"pointer", background:selTime===t?C.pink:C.white, color:selTime===t?"#fff":C.text, fontWeight:600, fontSize:14, border:`1px solid ${selTime===t?C.pink:C.border}` }}>{t}</button>
            ))}
          </div>
        </>
      )}

      <button onClick={handleBook} style={{ width:"100%", padding:"16px", borderRadius:16, border:"none", background:selTime&&selDate?C.pink:"#dde6f5", color:selTime&&selDate?"#fff":C.muted, fontSize:16, fontWeight:700, cursor:selTime&&selDate?"pointer":"not-allowed", marginBottom:12 }}>
        {selTime&&selDate?"💳 QPay-р төлж захиалах":"Огноо болон цаг сонгоно уу"}
      </button>

      <div style={{ background:"#fef9c3", border:"1px solid #fde047", borderRadius:14, padding:"12px 16px", textAlign:"center" }}>
        <div style={{ fontSize:13, color:"#854d0e" }}>🔒 Эмчийн дугаар нь <strong>төлбөр төлсний дараа</strong> харагдана</div>
      </div>
    </div>
  );
}

// ─── DOCTORS SCREEN ──────────────────────────────────────────────────────────
function DoctorsScreen({ onBook }) {
  const [selected, setSelected] = useState(null);
  if (selected) return <DoctorDetail doc={selected} onBack={()=>setSelected(null)} onBook={onBook}/>;
  return (
    <div style={{ padding:"20px 16px" }}>
      <div style={{ fontSize:20, fontWeight:800, color:C.text, marginBottom:6 }}>Эмч нар</div>
      <div style={{ fontSize:13, color:C.muted, marginBottom:20 }}>Төгсмед эмэгтэйчүүдийн эмнэлэг</div>
      <div style={{ display:"flex", flexDirection:"column", gap:12 }}>
        {DOCTORS.map(doc=>(
          <button key={doc.id} onClick={()=>setSelected(doc)} style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:18, padding:"16px", display:"flex", gap:14, alignItems:"center", cursor:"pointer", textAlign:"left", boxShadow:"0 2px 12px rgba(236,72,153,0.1)" }}>
            <img src={doc.img} style={{ width:64, height:64, borderRadius:50, objectFit:"cover", objectPosition:"center top", flexShrink:0, border:`2px solid ${C.border}` }} onError={e=>{e.target.style.background="#fdf2f8";}}/>
            <div style={{ flex:1 }}>
              <div style={{ display:"flex", alignItems:"center", gap:6, marginBottom:2 }}>
                <div style={{ fontSize:15, fontWeight:700, color:C.text }}>{doc.name}</div>
                <span style={{ fontSize:10, background:C.pinkLight, color:C.pink, borderRadius:99, padding:"2px 8px", fontWeight:700 }}>✓</span>
              </div>
              <div style={{ fontSize:12, color:C.pink, marginBottom:4 }}>{doc.specialty}</div>
              <StarRating rating={doc.rating}/>
              <div style={{ fontSize:11, color:C.muted, marginTop:4 }}>{doc.exp} туршлага · {doc.priceLabel}</div>
            </div>
            <div style={{ background:C.pinkLight, color:C.pink, borderRadius:10, padding:"6px 12px", fontSize:12, fontWeight:700 }}>Цаг авах</div>
          </button>
        ))}
      </div>
    </div>
  );
}

// ─── HOME SCREEN ─────────────────────────────────────────────────────────────
function HomeScreen({ setTab }) {
  return (
    <div>
      {/* Hero with logo */}
      <div style={{ background:"linear-gradient(135deg, #ec4899 0%, #a855f7 100%)", padding:"28px 20px 36px", borderRadius:"0 0 32px 32px" }}>
        <div style={{ display:"flex", alignItems:"center", gap:14, marginBottom:16 }}>
          <img src={LOGO} style={{ width:56, height:56, borderRadius:14, objectFit:"cover", background:"rgba(255,255,255,0.2)" }} onError={e=>e.target.style.display="none"}/>
          <div>
            <div style={{ fontSize:20, fontWeight:800, color:"#fff" }}>Төгсмед эмнэлэг</div>
            <div style={{ fontSize:12, color:"rgba(255,255,255,0.85)" }}>Эмэгтэйчүүдийн мэргэшсэн эмнэлэг</div>
          </div>
        </div>
        <div style={{ background:"rgba(255,255,255,0.15)", borderRadius:14, padding:"12px 16px", display:"flex", gap:10, alignItems:"center" }}>
          <span style={{ fontSize:18 }}>🔍</span>
          <span style={{ color:"rgba(255,255,255,0.8)", fontSize:14 }}>Эмч, өвчин хайх...</span>
        </div>
      </div>

      <div style={{ padding:"20px 16px" }}>
        {/* Quick actions */}
        <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:12, marginBottom:24 }}>
          {[
            { icon:"📅", label:"Цаг захиалах", color:C.pink, bg:C.pinkLight, action:"doctors" },
            { icon:"💬", label:"WhatsApp зөвлөгөө", color:"#25D366", bg:"#f0fdf4", action:"doctors" },
            { icon:"📋", label:"Эрүүл мэндийн мэдээлэл", color:"#f59e0b", bg:"#fff8e8", action:"health" },
            { icon:"💊", label:"Миний цагнууд", color:"#8b5cf6", bg:"#f3f0ff", action:"appointments" },
          ].map((a,i)=>(
            <button key={i} onClick={()=>setTab(a.action)} style={{ background:a.bg, border:"none", borderRadius:16, padding:"18px 14px", cursor:"pointer", textAlign:"left" }}>
              <div style={{ fontSize:28, marginBottom:8 }}>{a.icon}</div>
              <div style={{ fontSize:13, fontWeight:600, color:a.color }}>{a.label}</div>
            </button>
          ))}
        </div>

        {/* Doctors */}
        <div style={{ fontWeight:700, fontSize:16, color:C.text, marginBottom:12 }}>👩‍⚕️ Манай эмч нар</div>
        <div style={{ display:"flex", flexDirection:"column", gap:10, marginBottom:20 }}>
          {DOCTORS.map(doc=>(
            <div key={doc.id} onClick={()=>setTab("doctors")} style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:18, padding:"14px 16px", display:"flex", gap:14, alignItems:"center", cursor:"pointer", boxShadow:"0 2px 8px rgba(236,72,153,0.08)" }}>
              <img src={doc.img} style={{ width:56, height:56, borderRadius:50, objectFit:"cover", objectPosition:"center top", border:`2px solid ${C.border}` }} onError={e=>e.target.style.background="#fdf2f8"}/>
              <div style={{ flex:1 }}>
                <div style={{ fontSize:15, fontWeight:800, color:C.text }}>{doc.name}</div>
                <div style={{ fontSize:12, color:C.pink }}>{doc.specialty}</div>
                <StarRating rating={doc.rating}/>
              </div>
              <div style={{ background:C.pinkLight, color:C.pink, borderRadius:10, padding:"6px 12px", fontSize:12, fontWeight:700 }}>Цаг авах</div>
            </div>
          ))}
        </div>

        {/* Tip */}
        <div style={{ background:"linear-gradient(135deg, #fdf2f8, #fff)", borderRadius:16, padding:"16px", border:`1px solid ${C.border}`, marginBottom:20 }}>
          <div style={{ fontSize:12, color:C.pink, fontWeight:700, marginBottom:6 }}>💡 ӨНӨӨДРИЙН ЗӨВЛӨГӨӨ</div>
          <div style={{ fontSize:14, color:C.text, lineHeight:1.6 }}>Жил бүр <strong>эмэгтэйчүүдийн эмчид</strong> үзүүлж, Паап шинжилгээ өгөх нь эрүүл мэндийнхээ хамгийн чухал хөрөнгө оруулалт. 🌸</div>
        </div>

        {/* Clinic contact footer - only clinic number */}
        <div style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:20, padding:"18px 16px" }}>
          <div style={{ display:"flex", alignItems:"center", gap:12, marginBottom:14 }}>
            <img src={LOGO} style={{ width:40, height:40, borderRadius:10, objectFit:"cover" }} onError={e=>e.target.style.display="none"}/>
            <div>
              <div style={{ fontWeight:800, fontSize:15, color:C.text }}>Төгсмед эмэгтэйчүүдийн эмнэлэг</div>
              <div style={{ fontSize:11, color:C.muted }}>Мэргэжлийн онлайн зөвлөгөө</div>
            </div>
          </div>
          <a href={`tel:+976${CLINIC_PHONE}`}
            style={{ display:"flex", alignItems:"center", gap:12, background:C.pinkLight, borderRadius:14, padding:"14px 16px", textDecoration:"none" }}>
            <div style={{ width:44, height:44, borderRadius:12, background:C.pink, display:"flex", alignItems:"center", justifyContent:"center", fontSize:22 }}>📞</div>
            <div>
              <div style={{ fontSize:12, color:C.muted }}>Эмнэлгийн утас</div>
              <div style={{ fontSize:18, fontWeight:800, color:C.pink }}>{CLINIC_PHONE}</div>
            </div>
            <span style={{ marginLeft:"auto", color:C.pink, fontSize:20 }}>→</span>
          </a>
        </div>
      </div>
    </div>
  );
}

// ─── HEALTH SCREEN ───────────────────────────────────────────────────────────
function HealthScreen() {
  const [selected, setSelected] = useState(null);
  if (selected) return (
    <div style={{ padding:"20px 16px" }}>
      <button onClick={()=>setSelected(null)} style={{ background:"none", border:"none", fontSize:22, cursor:"pointer", marginBottom:12, color:C.pink }}>←</button>
      <div style={{ background:`${selected.color}15`, border:`1px solid ${selected.color}30`, borderRadius:20, padding:"20px", marginBottom:20 }}>
        <div style={{ fontSize:40, marginBottom:8 }}>{selected.icon}</div>
        <div style={{ fontSize:22, fontWeight:800, color:C.text }}>{selected.title}</div>
        <div style={{ fontSize:14, color:C.muted, marginTop:6, lineHeight:1.6 }}>{selected.summary}</div>
      </div>
      {selected.sections.map((s,i)=>(
        <div key={i} style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:16, padding:16, marginBottom:12 }}>
          <div style={{ fontWeight:700, color:selected.color, marginBottom:8 }}>{s.title}</div>
          <div style={{ fontSize:14, color:C.muted, lineHeight:1.8 }}>{s.content}</div>
        </div>
      ))}
    </div>
  );
  return (
    <div style={{ padding:"20px 16px" }}>
      <div style={{ fontSize:20, fontWeight:800, color:C.text, marginBottom:6 }}>Эрүүл мэндийн мэдээлэл</div>
      <div style={{ fontSize:13, color:C.muted, marginBottom:20 }}>Эмэгтэйчүүдийн эрүүл мэндийн мэдлэг</div>
      <div style={{ display:"flex", flexDirection:"column", gap:12 }}>
        {HEALTH_TOPICS.map(t=>(
          <button key={t.id} onClick={()=>setSelected(t)} style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:18, padding:"18px 16px", display:"flex", gap:14, alignItems:"center", cursor:"pointer", textAlign:"left" }}>
            <div style={{ width:52, height:52, borderRadius:14, flexShrink:0, background:`${t.color}18`, display:"flex", alignItems:"center", justifyContent:"center", fontSize:26 }}>{t.icon}</div>
            <div style={{ flex:1 }}>
              <div style={{ fontSize:15, fontWeight:700, color:C.text, marginBottom:4 }}>{t.title}</div>
              <div style={{ fontSize:12, color:C.muted }}>{t.summary.slice(0,55)}...</div>
            </div>
            <span style={{ color:C.muted, fontSize:18 }}>›</span>
          </button>
        ))}
      </div>
    </div>
  );
}

// ─── APPOINTMENTS SCREEN ─────────────────────────────────────────────────────
function AppointmentsScreen({ appointments, onCancel }) {
  const [showContact, setShowContact] = useState(null);
  if (!appointments.length) return (
    <div style={{ padding:"60px 16px", textAlign:"center" }}>
      <div style={{ fontSize:64, marginBottom:16 }}>📅</div>
      <div style={{ fontSize:18, fontWeight:700, color:C.text, marginBottom:8 }}>Цаг захиалаагүй байна</div>
      <div style={{ fontSize:14, color:C.muted }}>Эмч нар хэсэгт очиж цаг захиална уу</div>
    </div>
  );
  return (
    <div style={{ padding:"20px 16px" }}>
      {showContact && <ContactModal doc={showContact.doc} appointment={showContact} onClose={()=>setShowContact(null)}/>}
      <div style={{ fontSize:20, fontWeight:800, color:C.text, marginBottom:20 }}>Миний захиалгууд</div>
      <div style={{ display:"flex", flexDirection:"column", gap:12 }}>
        {appointments.map((a,i)=>{
          const dateStr = a.date ? `${a.date.getFullYear()}-${a.date.getMonth()+1}-${a.date.getDate()}` : "";
          return (
            <div key={i} style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:18, padding:16 }}>
              <div style={{ display:"flex", gap:12, alignItems:"center", marginBottom:12 }}>
                <img src={a.doc.img} style={{ width:50, height:50, borderRadius:50, objectFit:"cover", objectPosition:"center top" }}/>
                <div>
                  <div style={{ fontSize:15, fontWeight:700, color:C.text }}>{a.doc.name}</div>
                  <div style={{ fontSize:12, color:C.pink }}>{a.doc.specialty}</div>
                </div>
                <div style={{ marginLeft:"auto", background:"#e6faf5", color:"#00c48c", borderRadius:10, padding:"4px 10px", fontSize:11, fontWeight:700 }}>✓ Төлөгдсөн</div>
              </div>
              <div style={{ display:"flex", gap:8, marginBottom:10 }}>
                {[{l:"Огноо",v:dateStr},{l:"Цаг",v:a.time},{l:"Үнэ",v:a.doc.priceLabel}].map((r,j)=>(
                  <div key={j} style={{ flex:1, background:C.pinkLight, borderRadius:10, padding:"10px", textAlign:"center" }}>
                    <div style={{ fontSize:11, color:C.muted }}>{r.l}</div>
                    <div style={{ fontSize:13, fontWeight:700, color:C.pink }}>{r.v}</div>
                  </div>
                ))}
              </div>
              <button onClick={()=>setShowContact(a)} style={{ width:"100%", padding:"12px", borderRadius:12, border:"none", background:"#25D366", color:"#fff", fontSize:14, fontWeight:700, cursor:"pointer", marginBottom:8 }}>
                💬 Эмчтэй холбогдох
              </button>
              <button onClick={()=>onCancel(i)} style={{ width:"100%", padding:"10px", borderRadius:12, border:"1px solid #fecaca", background:"#fff5f5", color:"#ef4444", fontSize:13, fontWeight:600, cursor:"pointer" }}>Цуцлах</button>
            </div>
          );
        })}
      </div>
    </div>
  );
}

// ─── PROFILE SCREEN ──────────────────────────────────────────────────────────
function ProfileScreen() {
  const [notif, setNotif] = useState(true);
  return (
    <div style={{ padding:"20px 16px" }}>
      <div style={{ background:"linear-gradient(135deg, #ec4899, #a855f7)", borderRadius:24, padding:"28px 20px", textAlign:"center", marginBottom:20 }}>
        <img src={LOGO} style={{ width:60, height:60, borderRadius:14, objectFit:"cover", margin:"0 auto 12px", display:"block" }} onError={e=>e.target.style.display="none"}/>
        <div style={{ fontSize:18, fontWeight:800, color:"#fff" }}>Төгсмед эмнэлэг</div>
        <div style={{ fontSize:13, color:"rgba(255,255,255,0.8)" }}>Эмэгтэйчүүдийн мэргэшсэн эмнэлэг</div>
      </div>
      {["👤 Хувийн мэдээлэл","📋 Эрүүл мэндийн бичиг","💳 Төлбөрийн түүх","🔒 Нууцлал","❓ Тусламж"].map((item,i)=>(
        <div key={i} style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:14, padding:"16px", marginBottom:10, display:"flex", justifyContent:"space-between", alignItems:"center", cursor:"pointer" }}>
          <span style={{ fontSize:14, color:C.text }}>{item}</span>
          <span style={{ color:C.muted }}>›</span>
        </div>
      ))}
      <div style={{ background:C.white, border:`1px solid ${C.border}`, borderRadius:14, padding:"16px", marginBottom:20 }}>
        <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center" }}>
          <span style={{ fontSize:14, color:C.text }}>🔔 Мэдэгдэл</span>
          <div onClick={()=>setNotif(n=>!n)} style={{ width:44, height:24, borderRadius:99, cursor:"pointer", background:notif?C.pink:"#dde6f5", position:"relative", transition:"background 0.3s" }}>
            <div style={{ position:"absolute", top:3, left:notif?23:3, width:18, height:18, borderRadius:50, background:"#fff", transition:"left 0.3s" }}/>
          </div>
        </div>
      </div>
      <div style={{ textAlign:"center", fontSize:12, color:C.muted }}>Төгсмед эмэгтэйчүүдийн эмнэлэг · v1.0.0</div>
    </div>
  );
}

// ─── MAIN ────────────────────────────────────────────────────────────────────
export default function App() {
  const [tab, setTab] = useState("home");
  const [appointments, setAppointments] = useState([]);
  const [toast, setToast] = useState(null);

  const handleBook = appt => {
    setAppointments(prev=>[...prev, appt]);
    setToast(`✅ ${appt.doc.name} — цаг захиалагдлаа!`);
    setTimeout(()=>setToast(null), 3500);
  };

  const handleCancel = i => {
    setAppointments(prev=>prev.filter((_,idx)=>idx!==i));
    setToast("🗑️ Цаг цуцлагдлаа");
    setTimeout(()=>setToast(null), 2500);
  };

  return (
    <div style={{ maxWidth:430, margin:"0 auto", minHeight:"100vh", background:C.bg, fontFamily:"'Segoe UI', system-ui, sans-serif", position:"relative" }}>
      <div style={{ background:"linear-gradient(135deg, #ec4899, #a855f7)", padding:"10px 20px 8px", display:"flex", justifyContent:"space-between", fontSize:12, color:"rgba(255,255,255,0.9)" }}>
        <span>9:41</span><span>🔋 📶</span>
      </div>
      {toast && (
        <div style={{ position:"fixed", top:60, left:"50%", transform:"translateX(-50%)", background:"#0f1d35", color:"#fff", padding:"12px 20px", borderRadius:14, fontSize:13, zIndex:200, maxWidth:360, textAlign:"center", boxShadow:"0 8px 32px rgba(0,0,0,0.3)" }}>{toast}</div>
      )}
      <div style={{ paddingBottom:80, minHeight:"calc(100vh - 40px)", overflowY:"auto" }}>
        {tab==="home" && <HomeScreen setTab={setTab}/>}
        {tab==="doctors" && <DoctorsScreen onBook={handleBook}/>}
        {tab==="health" && <HealthScreen/>}
        {tab==="appointments" && <AppointmentsScreen appointments={appointments} onCancel={handleCancel}/>}
        {tab==="profile" && <ProfileScreen/>}
      </div>
      <BottomNav tab={tab} setTab={setTab}/>
      <style>{`* { box-sizing:border-box; } ::-webkit-scrollbar { width:0; }`}</style>
    </div>
  );
}
