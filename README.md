import { useState, useEffect, useRef } from "react";

const SKILLS = {
  languages: [
    { name: "Python", color: "#3776AB", icon: "🐍", level: 88 },
    { name: "TypeScript", color: "#3178C6", icon: "🔷", level: 82 },
    { name: "JavaScript", color: "#F7DF1E", icon: "⚡", level: 90 },
  ],
  frontend: [
    { name: "HTML5", color: "#E34F26", icon: "🌐", level: 85 },
    { name: "CSS3", color: "#1572B6", icon: "🎨", level: 80 },
    { name: "Node.js", color: "#339933", icon: "🟢", level: 78 },
    { name: "Bun", color: "#ff77ff", icon: "🐢", level: 70 },
  ],
  database: [
    { name: "MongoDB", color: "#47A248", icon: "🍃", level: 75 },
    { name: "MySQL", color: "#4479A1", icon: "🐬", level: 72 },
    { name: "MariaDB", color: "#003545", icon: "🐘", level: 68 },
  ],
};

const SOCIALS = [
  { name: "Facebook", url: "https://facebook.com/naiahtel", color: "#1877F2", symbol: "f" },
  { name: "YouTube", url: "https://www.youtube.com/@NaiahtelPenguin", color: "#FF0000", symbol: "▶" },
  { name: "TikTok", url: "https://tiktok.com/@LeThaiAn07", color: "#ffffff", symbol: "♪" },
  { name: "Instagram", url: "https://instagram.com/lethaian07", color: "#E4405F", symbol: "◈" },
  { name: "Discord", url: "https://discord.com/users/1197915568414654557", color: "#5865F2", symbol: "◉" },
];

function GlitchText({ text }) {
  const [glitch, setGlitch] = useState(false);
  useEffect(() => {
    const t = setInterval(() => {
      setGlitch(true);
      setTimeout(() => setGlitch(false), 200);
    }, 3000 + Math.random() * 2000);
    return () => clearInterval(t);
  }, []);
  return (
    <span style={{ position: "relative", display: "inline-block" }}>
      <span style={{
        color: glitch ? "#ff77ff" : "inherit",
        textShadow: glitch ? "2px 0 #00ffff, -2px 0 #ff4444" : "inherit",
        transition: "all 0.1s",
        letterSpacing: "0.05em",
      }}>{text}</span>
    </span>
  );
}

function SkillBar({ skill, delay }) {
  const [width, setWidth] = useState(0);
  const [hovered, setHovered] = useState(false);
  useEffect(() => {
    const t = setTimeout(() => setWidth(skill.level), 400 + delay * 80);
    return () => clearTimeout(t);
  }, [skill.level, delay]);
  return (
    <div
      onMouseEnter={() => setHovered(true)}
      onMouseLeave={() => setHovered(false)}
      style={{
        marginBottom: "10px",
        cursor: "default",
        transform: hovered ? "translateX(4px)" : "translateX(0)",
        transition: "transform 0.2s ease",
      }}
    >
      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: "5px" }}>
        <span style={{ fontSize: "12px", color: "#ccd6f6", fontFamily: "'Fira Code', monospace", display: "flex", alignItems: "center", gap: "6px" }}>
          <span>{skill.icon}</span>
          <span style={{ color: hovered ? skill.color : "#ccd6f6", transition: "color 0.2s" }}>{skill.name}</span>
        </span>
        <span style={{ fontSize: "11px", color: skill.color, fontFamily: "'Fira Code', monospace" }}>{skill.level}%</span>
      </div>
      <div style={{ height: "4px", background: "rgba(255,119,255,0.1)", borderRadius: "2px", overflow: "hidden", position: "relative" }}>
        <div style={{
          height: "100%",
          width: `${width}%`,
          background: `linear-gradient(90deg, ${skill.color}88, ${skill.color})`,
          borderRadius: "2px",
          transition: "width 1.2s cubic-bezier(0.4,0,0.2,1)",
          boxShadow: hovered ? `0 0 8px ${skill.color}` : "none",
          position: "relative",
        }}>
          {hovered && (
            <div style={{
              position: "absolute", right: 0, top: "-2px", width: "8px", height: "8px",
              borderRadius: "50%", background: skill.color,
              boxShadow: `0 0 6px ${skill.color}`,
            }} />
          )}
        </div>
      </div>
    </div>
  );
}

function Hexagon({ children, color, size = 48 }) {
  const [hovered, setHovered] = useState(false);
  return (
    <div
      onMouseEnter={() => setHovered(true)}
      onMouseLeave={() => setHovered(false)}
      style={{
        width: size, height: size,
        background: hovered ? `${color}22` : `${color}11`,
        border: `1px solid ${hovered ? color : color + "44"}`,
        borderRadius: "8px",
        display: "flex", alignItems: "center", justifyContent: "center",
        cursor: "pointer",
        transform: hovered ? "scale(1.15) translateY(-2px)" : "scale(1)",
        transition: "all 0.25s cubic-bezier(0.34,1.56,0.64,1)",
        boxShadow: hovered ? `0 0 16px ${color}55, 0 4px 20px ${color}22` : "none",
        position: "relative",
        overflow: "hidden",
      }}
    >
      {hovered && (
        <div style={{
          position: "absolute", top: 0, left: "-100%", width: "100%", height: "100%",
          background: `linear-gradient(90deg, transparent, ${color}33, transparent)`,
          animation: "shimmer 0.5s ease forwards",
        }} />
      )}
      {children}
    </div>
  );
}

function MatrixRain() {
  const canvasRef = useRef(null);
  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    const ctx = canvas.getContext("2d");
    canvas.width = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;
    const cols = Math.floor(canvas.width / 14);
    const drops = Array(cols).fill(0).map(() => Math.random() * -canvas.height);
    const chars = "アイウエオ01アイウエ010101PYTHON TYPESCRIPT JAVASCRIPT NODE BUN MONGO";
    let frame;
    const draw = () => {
      ctx.fillStyle = "rgba(10,10,26,0.06)";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
      ctx.font = "11px 'Fira Code', monospace";
      drops.forEach((y, i) => {
        const char = chars[Math.floor(Math.random() * chars.length)];
        const alpha = Math.random() * 0.3 + 0.05;
        ctx.fillStyle = `rgba(255,119,255,${alpha})`;
        ctx.fillText(char, i * 14, y);
        drops[i] = y > canvas.height + 20 ? Math.random() * -100 : y + 14;
      });
      frame = requestAnimationFrame(draw);
    };
    draw();
    return () => cancelAnimationFrame(frame);
  }, []);
  return (
    <canvas ref={canvasRef} style={{ position: "absolute", inset: 0, width: "100%", height: "100%", opacity: 0.4, pointerEvents: "none" }} />
  );
}

function SectionTitle({ children, accent = "#ff77ff" }) {
  return (
    <div style={{ display: "flex", alignItems: "center", gap: "10px", marginBottom: "18px", marginTop: "8px" }}>
      <div style={{ width: "3px", height: "18px", background: accent, borderRadius: "2px", boxShadow: `0 0 8px ${accent}` }} />
      <span style={{
        fontFamily: "'Fira Code', monospace", fontSize: "11px", fontWeight: 700,
        letterSpacing: "0.18em", textTransform: "uppercase",
        color: accent, textShadow: `0 0 12px ${accent}44`,
      }}>{children}</span>
      <div style={{ flex: 1, height: "1px", background: `linear-gradient(90deg, ${accent}44, transparent)` }} />
    </div>
  );
}

function StatCard({ label, value, color = "#ff77ff" }) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    let start = 0;
    const step = Math.ceil(value / 40);
    const t = setInterval(() => {
      start = Math.min(start + step, value);
      setCount(start);
      if (start >= value) clearInterval(t);
    }, 30);
    return () => clearInterval(t);
  }, [value]);
  return (
    <div style={{
      background: `linear-gradient(135deg, ${color}0a, ${color}05)`,
      border: `1px solid ${color}22`,
      borderRadius: "12px", padding: "14px 18px",
      textAlign: "center", flex: 1,
      transition: "border-color 0.3s",
    }}
    onMouseEnter={e => e.currentTarget.style.borderColor = color + "66"}
    onMouseLeave={e => e.currentTarget.style.borderColor = color + "22"}
    >
      <div style={{ fontSize: "22px", fontWeight: 800, color, fontFamily: "'Fira Code', monospace", textShadow: `0 0 16px ${color}66` }}>
        {count}
      </div>
      <div style={{ fontSize: "10px", color: "#8892b0", fontFamily: "'Fira Code', monospace", letterSpacing: "0.1em", marginTop: "4px" }}>
        {label}
      </div>
    </div>
  );
}

export default function App() {
  const [loaded, setLoaded] = useState(false);
  const [time, setTime] = useState(new Date());
  const [activeTab, setActiveTab] = useState("languages");

  useEffect(() => {
    setTimeout(() => setLoaded(true), 100);
    const t = setInterval(() => setTime(new Date()), 1000);
    return () => clearInterval(t);
  }, []);

  const allSkills = [...SKILLS.languages, ...SKILLS.frontend, ...SKILLS.database];

  return (
    <div style={{
      minHeight: "100vh", width: "100%",
      background: "radial-gradient(ellipse at 20% 50%, #0d0d1a 0%, #0a0a1a 40%, #050510 100%)",
      display: "flex", alignItems: "center", justifyContent: "center",
      fontFamily: "'Fira Code', monospace",
      padding: "20px",
      boxSizing: "border-box",
    }}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;500;600;700&family=Orbitron:wght@700;900&display=swap');
        * { box-sizing: border-box; }
        @keyframes shimmer { from { left: -100% } to { left: 100% } }
        @keyframes pulse { 0%,100% { opacity:1 } 50% { opacity:0.5 } }
        @keyframes scanline {
          0% { transform: translateY(-100%) }
          100% { transform: translateY(100vh) }
        }
        @keyframes float { 0%,100% { transform: translateY(0) } 50% { transform: translateY(-6px) } }
        @keyframes borderSpin {
          0% { background-position: 0% 50% }
          50% { background-position: 100% 50% }
          100% { background-position: 0% 50% }
        }
        @keyframes fadeSlideIn {
          from { opacity:0; transform:translateY(24px) }
          to { opacity:1; transform:translateY(0) }
        }
        ::-webkit-scrollbar { width: 4px }
        ::-webkit-scrollbar-track { background: #0a0a1a }
        ::-webkit-scrollbar-thumb { background: #ff77ff44; border-radius: 2px }
      `}</style>

      {/* Scanline overlay */}
      <div style={{
        position: "fixed", inset: 0, pointerEvents: "none", zIndex: 100, overflow: "hidden",
      }}>
        <div style={{
          position: "absolute", left: 0, right: 0, height: "2px",
          background: "linear-gradient(transparent, rgba(255,119,255,0.06), transparent)",
          animation: "scanline 4s linear infinite",
        }} />
      </div>

      {/* Main card */}
      <div style={{
        width: "100%", maxWidth: "680px",
        background: "rgba(10,10,30,0.95)",
        border: "1px solid rgba(255,119,255,0.2)",
        borderRadius: "20px",
        overflow: "hidden", position: "relative",
        boxShadow: "0 0 60px rgba(255,119,255,0.08), 0 40px 80px rgba(0,0,0,0.6)",
        opacity: loaded ? 1 : 0,
        transform: loaded ? "translateY(0)" : "translateY(30px)",
        transition: "all 0.8s cubic-bezier(0.34,1.56,0.64,1)",
        animation: loaded ? "none" : undefined,
      }}>

        {/* Hero Header */}
        <div style={{
          position: "relative", overflow: "hidden",
          background: "linear-gradient(135deg, #0a0014 0%, #120020 40%, #0a0014 100%)",
          padding: "40px 36px 32px",
          borderBottom: "1px solid rgba(255,119,255,0.12)",
        }}>
          <MatrixRain />

          {/* Corner decorations */}
          {[["top","left"],["top","right"],["bottom","left"],["bottom","right"]].map(([v,h]) => (
            <div key={`${v}${h}`} style={{
              position:"absolute", [v]: 12, [h]: 12,
              width: 16, height: 16,
              borderTop: v==="top" ? "2px solid #ff77ff" : "none",
              borderBottom: v==="bottom" ? "2px solid #ff77ff" : "none",
              borderLeft: h==="left" ? "2px solid #ff77ff" : "none",
              borderRight: h==="right" ? "2px solid #ff77ff" : "none",
              opacity: 0.6,
            }} />
          ))}

          {/* Status bar */}
          <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:"24px", position:"relative", zIndex:2 }}>
            <div style={{ display:"flex", alignItems:"center", gap:"6px" }}>
              <div style={{ width:6, height:6, borderRadius:"50%", background:"#00ff88", boxShadow:"0 0 6px #00ff88", animation:"pulse 2s infinite" }} />
              <span style={{ fontSize:"9px", color:"#00ff88", letterSpacing:"0.15em" }}>ONLINE</span>
            </div>
            <span style={{ fontSize:"9px", color:"#8892b0", letterSpacing:"0.1em" }}>
              {time.toLocaleTimeString("vi-VN", { hour12: false })} ICT
            </span>
          </div>

          {/* Avatar + Name */}
          <div style={{ display:"flex", alignItems:"center", gap:"24px", position:"relative", zIndex:2 }}>
            {/* Avatar */}
            <div style={{ position:"relative", flexShrink:0 }}>
              <div style={{
                width:80, height:80, borderRadius:"50%",
                background:"linear-gradient(135deg, #ff77ff, #7733ff, #00ccff)",
                padding:"2px",
                animation:"float 4s ease-in-out infinite",
              }}>
                <div style={{
                  width:"100%", height:"100%", borderRadius:"50%",
                  background:"linear-gradient(135deg, #1a0030, #0a1030)",
                  display:"flex", alignItems:"center", justifyContent:"center",
                  fontSize:"28px",
                }}>🐧</div>
              </div>
              <div style={{
                position:"absolute", bottom:0, right:0,
                width:20, height:20, borderRadius:"50%",
                background:"linear-gradient(135deg, #ff77ff, #7733ff)",
                display:"flex", alignItems:"center", justifyContent:"center",
                fontSize:"10px", border:"2px solid #0a0a1a",
              }}>⚡</div>
            </div>

            {/* Name & bio */}
            <div>
              <div style={{ fontSize:"11px", color:"#ff77ff99", letterSpacing:"0.2em", marginBottom:"4px" }}>
                &gt; identify user
              </div>
              <h1 style={{
                margin:0,
                fontSize:"clamp(22px, 5vw, 32px)",
                fontFamily:"'Orbitron', monospace",
                fontWeight:900,
                background:"linear-gradient(135deg, #ffffff 0%, #ff77ff 50%, #aa44ff 100%)",
                WebkitBackgroundClip:"text",
                WebkitTextFillColor:"transparent",
                backgroundClip:"text",
                letterSpacing:"0.04em",
                lineHeight:1.1,
              }}>
                <GlitchText text="LÊ THÁI AN" />
              </h1>
              <div style={{ fontSize:"11px", color:"#8892b0", marginTop:"6px", letterSpacing:"0.08em" }}>
                <span style={{ color:"#ff77ff" }}>@</span>NaiahtelPenguin
                <span style={{ color:"#ffffff22", margin:"0 6px" }}>|</span>
                <span style={{ color:"#00ccff" }}>FullStack Dev</span>
              </div>
            </div>
          </div>

          {/* Tags */}
          <div style={{ display:"flex", gap:"8px", flexWrap:"wrap", marginTop:"20px", position:"relative", zIndex:2 }}>
            {["🎮 Gamer","💻 Dev","🤖 Builder","🌙 Night Coder","🐧 Penguin Lover"].map(tag => (
              <span key={tag} style={{
                padding:"3px 10px", borderRadius:"20px",
                background:"rgba(255,119,255,0.08)",
                border:"1px solid rgba(255,119,255,0.2)",
                fontSize:"10px", color:"#ccd6f6",
                letterSpacing:"0.05em",
              }}>{tag}</span>
            ))}
          </div>
        </div>

        {/* Content area */}
        <div style={{ padding:"28px 36px" }}>

          {/* Socials */}
          <div style={{ animation:"fadeSlideIn 0.6s ease 0.1s both" }}>
            <SectionTitle>Socials</SectionTitle>
            <div style={{ display:"flex", gap:"10px", flexWrap:"wrap", marginBottom:"28px" }}>
              {SOCIALS.map(s => (
                <a key={s.name} href={s.url} target="_blank" rel="noopener noreferrer" style={{ textDecoration:"none" }}>
                  <Hexagon color={s.color} size={44}>
                    <span style={{ fontSize:"18px", color:s.color, fontWeight:700 }}>{s.symbol}</span>
                  </Hexagon>
                </a>
              ))}
            </div>
          </div>

          {/* Stats */}
          <div style={{ animation:"fadeSlideIn 0.6s ease 0.2s both" }}>
            <SectionTitle>Stats</SectionTitle>
            <div style={{ display:"flex", gap:"10px", marginBottom:"28px", flexWrap:"wrap" }}>
              <StatCard label="REPOS" value={24} color="#ff77ff" />
              <StatCard label="COMMITS" value={312} color="#00ccff" />
              <StatCard label="STARS" value={48} color="#ffcc00" />
              <StatCard label="PRs" value={17} color="#00ff88" />
            </div>
          </div>

          {/* Skills */}
          <div style={{ animation:"fadeSlideIn 0.6s ease 0.3s both" }}>
            <SectionTitle>Skills</SectionTitle>

            {/* Tab selector */}
            <div style={{ display:"flex", gap:"4px", marginBottom:"18px", background:"rgba(255,119,255,0.05)", borderRadius:"8px", padding:"4px" }}>
              {[
                { key:"languages", label:"Languages" },
                { key:"frontend", label:"Frontend/Backend" },
                { key:"database", label:"Database" },
              ].map(tab => (
                <button key={tab.key} onClick={() => setActiveTab(tab.key)} style={{
                  flex:1, padding:"6px 8px",
                  background: activeTab===tab.key ? "rgba(255,119,255,0.18)" : "transparent",
                  border: activeTab===tab.key ? "1px solid rgba(255,119,255,0.4)" : "1px solid transparent",
                  borderRadius:"6px",
                  color: activeTab===tab.key ? "#ff77ff" : "#8892b0",
                  fontSize:"10px", fontFamily:"'Fira Code', monospace",
                  letterSpacing:"0.05em", cursor:"pointer",
                  transition:"all 0.2s",
                }}>
                  {tab.label}
                </button>
              ))}
            </div>

            <div style={{ minHeight:"140px" }}>
              {SKILLS[activeTab].map((skill, i) => (
                <SkillBar key={skill.name} skill={skill} delay={i} />
              ))}
            </div>
          </div>

          {/* GitHub image embed */}
          <div style={{ animation:"fadeSlideIn 0.6s ease 0.4s both", marginTop:"28px" }}>
            <SectionTitle accent="#00ccff">GitHub Activity</SectionTitle>
            <div style={{
              borderRadius:"12px", overflow:"hidden",
              border:"1px solid rgba(0,204,255,0.15)",
              background:"rgba(0,204,255,0.03)",
              padding:"2px",
            }}>
              <img
                src="https://github-readme-stats.vercel.app/api?username=LeThaiAn07&show_icons=true&theme=rose&count_private=true&border_color=ff77ff&bg_color=0d1117&title_color=ff77ff&text_color=ccd6f6&icon_color=00ccff"
                alt="GitHub Stats"
                style={{ width:"100%", display:"block", borderRadius:"10px" }}
              />
            </div>
            <div style={{ marginTop:"10px", borderRadius:"12px", overflow:"hidden", border:"1px solid rgba(0,204,255,0.15)", background:"rgba(0,204,255,0.03)", padding:"2px" }}>
              <img
                src="https://github-readme-stats.vercel.app/api/top-langs/?username=LeThaiAn07&layout=compact&theme=rose&border_color=ff77ff&bg_color=0d1117&title_color=ff77ff&text_color=ccd6f6"
                alt="Top Languages"
                style={{ width:"100%", display:"block", borderRadius:"10px" }}
              />
            </div>
          </div>

          {/* Footer */}
          <div style={{
            marginTop:"28px", paddingTop:"20px",
            borderTop:"1px solid rgba(255,119,255,0.1)",
            display:"flex", justifyContent:"space-between", alignItems:"center",
            animation:"fadeSlideIn 0.6s ease 0.5s both",
          }}>
            <div style={{ display:"flex", alignItems:"center", gap:"6px" }}>
              <div style={{ width:4, height:4, borderRadius:"50%", background:"#ff77ff", animation:"pulse 1.5s infinite" }} />
              <span style={{ fontSize:"9px", color:"#8892b0", letterSpacing:"0.1em" }}>PROFILE VIEWS</span>
              <span style={{ fontSize:"9px", color:"#ff77ff", fontWeight:700 }}>∞</span>
            </div>
            <div style={{ fontSize:"9px", color:"#8892b0", letterSpacing:"0.08em" }}>
              LeThaiAn07 <span style={{ color:"#ff77ff" }}>✦</span> 2025
            </div>
          </div>
        </div>

        {/* Bottom accent bar */}
        <div style={{
          height:"3px",
          background:"linear-gradient(90deg, transparent, #7733ff, #ff77ff, #00ccff, transparent)",
          backgroundSize:"200% 100%",
          animation:"borderSpin 3s linear infinite",
        }} />
      </div>
    </div>
  );
}
