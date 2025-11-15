import React, { useEffect, useMemo, useState } from "react";
import { motion, useScroll, useSpring, useMotionValue, useTransform } from "framer-motion";
import { ArrowRight, Award, Building2, Calendar, ExternalLink, FileDown, LineChart, Linkedin, Mail, MapPin, Phone, Rocket, Sparkles, Star, X } from "lucide-react";

// Inline minimal UI primitives (to ensure preview works without external deps)
function Button({ variant = "default", size = "md", className = "", children, ...props }) {
  const base = "inline-flex items-center justify-center rounded-2xl font-medium transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2";
  const sizes = { sm: "h-8 px-3 text-sm", md: "h-10 px-4 text-sm", lg: "h-11 px-5 text-base" };
  const variants = {
    default: "bg-black text-white hover:bg-zinc-800 focus:ring-zinc-400",
    outline: "border border-zinc-300 bg-white/50 backdrop-blur hover:bg-white/70 focus:ring-zinc-400",
    secondary: "bg-white/60 backdrop-blur hover:bg-white/80 focus:ring-zinc-400",
  };
  return (
    <button className={`${base} ${sizes[size]} ${variants[variant]} ${className}`} {...props}>
      {children}
    </button>
  );
}

function Card({ className = "", children }) {
  return (
    <div className={`rounded-2xl border border-zinc-200 bg-white/60 backdrop-blur shadow-sm transition-transform duration-300 hover:-translate-y-1 hover:border-indigo-400/60 hover:shadow-xl hover:bg-gradient-to-r from-indigo-50 via-cyan-50 to-purple-50 ${className}`}>
      {children}
    </div>
  );
}
function CardHeader({ className = "", children }) { return (<div className={`px-6 pt-6 ${className}`}>{children}</div>); }
function CardTitle({ className = "", children }) { return (<h3 className={`text-lg font-semibold ${className}`}>{children}</h3>); }
function CardContent({ className = "", children }) { return (<div className={`px-6 pb-6 ${className}`}>{children}</div>); }
function Badge({ className = "", children, variant = "secondary" }) {
  const styles = variant === "secondary" ? "bg-zinc-100 text-zinc-800" : "bg-black text-white";
  return (<span className={`inline-flex items-center rounded-full px-3 py-1 text-xs ${styles} ${className}`}>{children}</span>);
}
function Avatar({ className = "", children }) { return (<div className={`relative inline-flex items-center justify-center overflow-hidden rounded-full bg-zinc-100 ${className}`}>{children}</div>); }
function AvatarImage({ src = "", alt = "" }) { return (<img src={src} alt={alt} className="h-full w-full object-cover" />); }
function AvatarFallback({ children }) { return (<span className="text-xs font-medium text-zinc-600">{children}</span>); }

// ---
// Personal Portfolio — Single-file React component
// Styling: TailwindCSS + shadcn/ui + lucide-react + framer-motion
// Replace placeholders as needed. This component is intentionally self-contained.
// ---

const DATA = {
  metaTitle: "Lokesh Sharma | AI Talent & AIML Program Leader | Scaler Portfolio",
  name: "Lokesh Sharma",
  title: "Team Lead - Advanced AIML Program",
  location: "Gurugram, India",
  email: "sharmalokesh500@outlook.com",
  phone: "+91 75501 67010",
  linkedin: "https://www.linkedin.com/in/sharmaloki/",
  resumeUrl: "#", // You can replace with an uploaded file URL
  summary:
    "Leader in AI/ML workforce transformation. I build high-integrity consulting systems, enable senior-tech talent to transition into AI roles, and partner with GTM to drive measurable business impact.",
  highlights: [
    { label: "Performance Uplift", value: "85%", icon: LineChart },
    { label: "Efficiency Gain", value: "96%", icon: Rocket },
    { label: "Revenue Influence", value: "$50M+", icon: Building2 },
    { label: "Career Transitions", value: "198", icon: Award },
  ],
  experience: [
    {
      role: "Team Lead – Advanced AIML Program",
      org: "Scaler by InterviewBit",
      period: "Feb 2022 – Present",
      location: "Gurugram, HR",
      points: [
        "Led 15 Technical Consultants; built consulting playbooks and enablement systems that drove an 85% performance uplift and global top conversion performance in FY 2024–25.",
        "Delivered 50+ enablement sessions on advisory frameworks, probing models, and AI capability mapping; improved lead-to-enrollment efficiency by 96% and maintained 86% conversions in senior cohorts.",
        "Advised 200+ tech professionals (avg. 15 yrs exp.), enabling 198 confirmed transitions into DS/ML/AI roles with a 95% success rate.",
        "Influenced $50M+ in revenue through structured consulting pipelines and long-term talent-readiness plans; maintained ~$4K ARPC.",
        "Founding contributor to Delhi-NCR expansion; core contributor to Scaler’s Advanced AIML (with IIT Roorkee) and DSML launches; contributed insights to Online MBA initiatives with IIM & Woolf University.",
        "Awards: Retention Star 2025, Rockstar Performer 2024, Glory Awards 2022 & 2023; consistently Top 3 globally for integrity-led advisory and outcomes.",
      ],
    },
    {
      role: "Product Consultant",
      org: "Lido",
      period: "Apr 2021 – Feb 2022",
      location: "Bengaluru, KA",
      points: [
        "Acquired and onboarded 250+ learners in K‑12 programs; contributed $36K+ ARR during high-growth phase.",
        "Personalized learning paths for parents and schools; strengthened retention and referrals.",
        "Ran micro-targeted social campaigns; accelerated organic lead inflow and earned internal recognition.",
      ],
    },
    {
      role: "Product Associate",
      org: "Byju’s",
      period: "Aug 2020 – Mar 2021",
      location: "Bengaluru, KA",
      points: [
        "Supported national JEE/UPSC/CAT campaigns; achieved 70–80% of weekly targets through lockdown volatility.",
        "Selected into a 12‑member cross‑functional leadership pod to streamline qualification and conversions.",
        "Fast‑tracked from Trainee to Associate; delivered ₹25L+ in training period; recognized for pipeline rigor.",
      ],
    },
  ],
  projects: [
    {
      title: "AI Workforce Transformation",
      org: "Scaler × IIT Roorkee",
      period: "2023 – Present",
      bullets: [
        "Co‑launched Advanced AIML program with IIT Roorkee.",
        "Drove AI skill adoption across mid‑senior workforce (10–25 yrs exp.).",
        "Consulted leaders on upskilling roadmaps and AI capability building.",
      ],
    },
    {
      title: "Sales Excellence & GTM Strategy",
      org: "Scaler",
      period: "Ongoing",
      bullets: [
        "Institutionalized consultative qualification frameworks for enterprise‑grade conversions.",
        "Engineered CRM accountability systems to increase engagement velocity and revenue predictability.",
        "Designed micro‑segmented outreach playbooks; improved GTM precision and program adoption.",
      ],
    },
  ],
  skills: [
    {
      category: "Business & Product",
      items: [
        "Product Thinking",
        "GTM Strategy",
        "Customer Success & Retention",
        "Business Analytics",
        "Solution Consulting",
        "Market & Competitive Research",
      ],
    },
    {
      category: "Consulting & Leadership",
      items: [
        "Strategic Advisory",
        "Stakeholder Management",
        "Performance Coaching",
        "Team Leadership",
        "Executive Career Advisory",
        "Change Enablement",
        "Enterprise AI Readiness",
      ],
    },
    {
      category: "Tools & Platforms",
      items: [
        "Scaler CRM",
        "LinkedIn",
        "Slack",
        "ChatGPT",
        "Perplexity",
        "DeepSeek",
        "Google Gemini",
        "Claude",
        "Notion",
        "Google Suite",
        "Microsoft Office Suite",
        "Tableau",
        "Google AI Studio",
      ],
    },
    {
      category: "Methodologies",
      items: [
        "Consultative Selling",
        "Needs Assessment",
        "Talent Gap Analysis",
        "Outcome‑Based Learning Design",
        "Program Management",
        "Growth Enablement Playbooks",
      ],
    },
  ],
  education: [
    {
      school: "Scaler Academy",
      place: "Bengaluru, KA",
      credential: "Data Science & Machine Learning Certification",
      period: "May 2025 – Present",
    },
    {
      school: "SRM Institute of Science and Technology",
      place: "Chennai, TN",
      credential: "B.Tech in Biotechnology",
      period: "Apr 2016 – May 2020",
    },
    {
      school: "Harvard University (HMX)",
      place: "Boston, MA",
      credential: "Certification in Genetic Engineering",
      period: "May 2018",
    },
  ],
};

// Utility: join classes
function cx(...classes) {
  return classes.filter(Boolean).join(" ");
}

function useThemeToggle() {
  // Force light mode only
  const [isDark, setIsDark] = useState(false);
  useEffect(() => {
    const root = document.documentElement;
    root.classList.remove("dark");
    try { localStorage.setItem("theme:dark", "0"); } catch {}
  }, []);
  return { isDark: false, setIsDark: () => {} };
}

// Scroll progress bar component
function ScrollProgressBar() {
  const { scrollYProgress } = useScroll();
  const scaleX = useSpring(scrollYProgress, { stiffness: 120, damping: 30, restDelta: 0.001 });
  useEffect(() => { document.title = DATA.metaTitle; }, []);

  return (
    <motion.div className="fixed left-0 top-0 z-40 h-1 w-full origin-left bg-indigo-500" style={{ scaleX }} />
  );
}

// Cursor glow that follows the mouse for a premium, interactive feel
function CursorGlow() {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  const [hue, setHue] = useState(210);
  useEffect(() => {
    const onMove = (e) => {
      setPos({ x: e.clientX, y: e.clientY });
      const w = window.innerWidth || 1;
      setHue(Math.round((e.clientX / w) * 360));
    };
    window.addEventListener("mousemove", onMove);
    return () => window.removeEventListener("mousemove", onMove);
  }, []);
  return (
    <motion.div
      aria-hidden
      className="pointer-events-none fixed z-10 h-72 w-72 -translate-x-1/2 -translate-y-1/2 rounded-full blur-3xl"
      animate={{ x: pos.x, y: pos.y }}
      transition={{ type: "spring", stiffness: 120, damping: 20, mass: 0.5 }}
      style={{
        background: `radial-gradient(circle at center,
          hsla(${hue}, 95%, 62%, 0.28),
          hsla(${(hue + 40) % 360}, 85%, 55%, 0.18),
          hsla(${(hue + 80) % 360}, 80%, 50%, 0.12))`,
      }}
    />
  );
}

// Subtle grain overlay for depth without noise
function BackgroundGrain() {
  return (
    <div
      aria-hidden
      className="pointer-events-none fixed inset-0 -z-10 opacity-[0.08] mix-blend-overlay"
      style={{
        backgroundImage:
          "radial-gradient(circle at 25% 25%, rgba(0,0,0,0.6) 0.5px, transparent 0.6px), radial-gradient(circle at 75% 75%, rgba(0,0,0,0.6) 0.5px, transparent 0.6px)",
        backgroundSize: "3px 3px, 3px 3px",
      }}
    />
  );
}

// Subtle AI neural grid (lines + soft pulses)
function NeuralGridBackground() {
  return (
    <div aria-hidden className="pointer-events-none fixed inset-0 -z-10">
      <div
        className="absolute inset-0 opacity-[0.06]"
        style={{
          backgroundImage:
            "linear-gradient(rgba(0,0,0,0.12) 1px, transparent 1px), linear-gradient(90deg, rgba(0,0,0,0.12) 1px, transparent 1px)",
          backgroundSize: "48px 48px, 48px 48px",
          backgroundPosition: "-1px -1px, -1px -1px",
        }}
      />
      {/* Soft node pulses */}
      <motion.div className="absolute left-[15%] top-[30%] h-2 w-2 rounded-full bg-cyan-400/60 blur-[1px]"
        animate={{ opacity: [0.2, 0.9, 0.2] }} transition={{ duration: 3, repeat: Infinity }} />
      <motion.div className="absolute left-[60%] top-[55%] h-2 w-2 rounded-full bg-indigo-400/60 blur-[1px]"
        animate={{ opacity: [0.2, 0.9, 0.2] }} transition={{ duration: 3.6, repeat: Infinity }} />
      <motion.div className="absolute left-[80%] top-[25%] h-2 w-2 rounded-full bg-purple-400/60 blur-[1px]"
        animate={{ opacity: [0.2, 0.9, 0.2] }} transition={{ duration: 4, repeat: Infinity }} />
    </div>
  );
}

// Counter text that animates numbers with optional prefix/suffix (e.g., $50M+, 85%)
function CounterSpan({ value }) {
  const extract = (v) => {
    const m = String(v).match(/^(\D*)([0-9]*\.?[0-9]+)(.*)$/);
    if (!m) return { prefix: "", num: 0, suffix: String(v) };
    return { prefix: m[1] ?? "", num: parseFloat(m[2] ?? 0), suffix: m[3] ?? "" };
  };
  const { prefix, num, suffix } = extract(value);
  const mv = useMotionValue(0);
  const spring = useSpring(mv, { stiffness: 120, damping: 20 });
  const [txt, setTxt] = useState("0");
  useEffect(() => {
    mv.set(0);
    const unsub = spring.on("change", (v) => {
      const rounded = Number.isInteger(num) ? Math.round(v) : Math.round(v * 10) / 10;
      setTxt(`${prefix}${rounded}${suffix}`);
    });
    const t = setTimeout(() => mv.set(num), 80);
    return () => { unsub?.(); clearTimeout(t); };
  }, [value]);
  return <span className="tabular-nums">{txt}</span>;
}

const Section = ({ id, title, children, accent = false }) => (
  <section id={id} className={cx("scroll-mt-24 py-14 lg:py-20", accent && "bg-zinc-50/40 dark:bg-zinc-900/30")}> 
    <div className="mx-auto max-w-6xl px-6">
      <motion.h2
        initial={{ opacity: 0, y: 12 }}
        whileInView={{ opacity: 1, y: 0 }}
        viewport={{ once: true }}
        transition={{ duration: 0.4 }}
        className="mb-8 text-2xl font-semibold tracking-tight sm:text-3xl lg:text-4xl"
      >
        {title}
      </motion.h2>
      {children}
    </div>
  </section>
);

const Metric = ({ icon: Icon, value, label }) => (
  <motion.div
    initial={{ opacity: 0, y: 10 }}
    whileInView={{ opacity: 1, y: 0 }}
    viewport={{ once: true }}
    transition={{ duration: 0.35 }}
  >
    <Card className="h-full">
      <CardContent className="flex items-center gap-4 p-5">
        <div className="rounded-2xl bg-indigo-500/10 p-3">
          <Icon className="h-6 w-6" />
        </div>
        <div>
          <div className="text-2xl font-bold leading-tight"><CounterSpan value={value} /></div>
          <div className="text-sm text-muted-foreground">{label}</div>
        </div>
      </CardContent>
    </Card>
  </motion.div>
);

const TimelineItem = ({ role, org, period, location, points }) => (
  <motion.div
    className="relative pl-8"
    initial={{ opacity: 0, y: 12 }}
    whileInView={{ opacity: 1, y: 0 }}
    viewport={{ once: true }}
    transition={{ duration: 0.4 }}
  >
    <div className="absolute left-0 top-2 h-3 w-3 -translate-x-1/2 rounded-full bg-indigo-500 shadow" />
    <Card className="mb-6">
      <CardHeader>
        <CardTitle className="flex flex-wrap items-center justify-between gap-2 text-lg">
          <span className="font-semibold">{role}</span>
          <span className="text-sm font-normal text-muted-foreground flex items-center gap-2">
            <Calendar className="h-4 w-4" /> {period}
          </span>
        </CardTitle>
        <div className="mt-1 flex flex-wrap items-center gap-3 text-sm text-muted-foreground">
          <span className="inline-flex items-center gap-2"><Building2 className="h-4 w-4" /> {org}</span>
          <span className="inline-flex items-center gap-2"><MapPin className="h-4 w-4" /> {location}</span>
        </div>
      </CardHeader>
      <CardContent>
        <ul className="space-y-2 text-[15px] leading-relaxed">
          {points.map((p, idx) => (
            <li key={idx} className="flex gap-2">
              <Star className="mt-1 h-4 w-4 flex-none opacity-60" />
              <span>{p}</span>
            </li>
          ))}
        </ul>
      </CardContent>
    </Card>
  </motion.div>
);

const ProjectCard = ({ title, org, period, bullets }) => (
  <motion.div
    initial={{ opacity: 0, y: 12 }}
    whileInView={{ opacity: 1, y: 0 }}
    viewport={{ once: true }}
    transition={{ duration: 0.35 }}
  >
    <Card className="h-full">
      <CardHeader>
        <CardTitle className="flex items-center justify-between text-lg">
          <span className="font-semibold">{title}</span>
          <span className="text-sm font-normal text-muted-foreground flex items-center gap-2"><Calendar className="h-4 w-4" /> {period}</span>
        </CardTitle>
        <div className="mt-1 text-sm text-muted-foreground"><Building2 className="mr-2 inline-block h-4 w-4" />{org}</div>
      </CardHeader>
      <CardContent>
        <ul className="space-y-2 text-[15px] leading-relaxed">
          {bullets.map((b, i) => (
            <li key={i} className="flex gap-2">
              <Sparkles className="mt-1 h-4 w-4 flex-none opacity-60" />
              <span>{b}</span>
            </li>
          ))}
        </ul>
      </CardContent>
    </Card>
  </motion.div>
);

const SkillPills = ({ category, items }) => (
  <Card>
    <CardHeader>
      <CardTitle className="text-base font-semibold">{category}</CardTitle>
    </CardHeader>
    <CardContent className="flex flex-wrap gap-2">
      {items.map((s, i) => (
        <Badge key={i} variant="secondary" className="rounded-full px-3 py-1 text-[13px]">
          {s}
        </Badge>
      ))}
    </CardContent>
  </Card>
);

const NavLink = ({ href, children, active = false, onClick }) => (
  <a
    href={href}
    onClick={onClick}
    className={cx(
      "relative text-sm font-medium transition-colors",
      active ? "text-zinc-900" : "text-muted-foreground hover:text-foreground"
    )}
  >
    {children}
    {active && (
      <motion.span
        layoutId="nav-underline"
        className="absolute -bottom-1 left-0 h-0.5 w-full rounded bg-zinc-900"
      />
    )}
  </a>
);

// 3D tilt wrapper
function TiltCard({ children }) {
  const ref = React.useRef(null);
  const px = useMotionValue(0.5);
  const py = useMotionValue(0.5);
  const rotateX = useTransform(py, [0, 1], [12, -12]);
  const rotateY = useTransform(px, [0, 1], [-12, 12]);
  const handleMove = (e) => {
    const el = ref.current;
    if (!el) return;
    const rect = el.getBoundingClientRect();
    const x = (e.clientX - rect.left) / rect.width;
    const y = (e.clientY - rect.top) / rect.height;
    px.set(x);
    py.set(y);
  };
  const reset = () => { px.set(0.5); py.set(0.5); };
  return (
    <motion.div
      ref={ref}
      className="[perspective:1000px]"
      onMouseMove={handleMove}
      onMouseLeave={reset}
      style={{ rotateX, rotateY, transformStyle: "preserve-3d", willChange: "transform" }}
      whileHover={{ scale: 1.015 }}
      transition={{ type: "spring", stiffness: 220, damping: 18 }}
    >
      {children}
    </motion.div>
  );
}

// Magnetic CTA wrapper
function Magnetic({ children }) {
  const ref = React.useRef(null);
  const tx = useMotionValue(0);
  const ty = useMotionValue(0);
  const onMove = (e) => {
    const el = ref.current;
    if (!el) return;
    const r = el.getBoundingClientRect();
    const x = e.clientX - (r.left + r.width / 2);
    const y = e.clientY - (r.top + r.height / 2);
    tx.set(x * 0.15);
    ty.set(y * 0.15);
  };
  const onLeave = () => { tx.set(0); ty.set(0); };
  return (
    <motion.div ref={ref} onMouseMove={onMove} onMouseLeave={onLeave} style={{ x: tx, y: ty }}>
      {children}
    </motion.div>
  );
}

// Animated word cycle component
function AnimatedWords({ words }) {
  const [index, setIndex] = React.useState(0);
  useEffect(() => {
    const id = setInterval(() => {
      setIndex((i) => (i + 1) % words.length);
    }, 1600);
    return () => clearInterval(id);
  }, [words.length]);
  return (
    <span className="relative inline-block">
      <motion.span
        key={index}
        initial={{ opacity: 0, y: 6 }}
        animate={{ opacity: 1, y: 0 }}
        exit={{ opacity: 0, y: -6 }}
        transition={{ duration: 0.38, ease: "easeOut" }}
        className="bg-gradient-to-r from-indigo-600 via-cyan-500 to-purple-500 bg-clip-text text-transparent font-extrabold"
      >
        {words[index]}
      </motion.span>
      {/* Underline sweep */}
      <motion.span
        className="absolute left-0 -bottom-1 h-[2px] w-full bg-gradient-to-r from-indigo-600 via-cyan-500 to-purple-500"
        initial={{ scaleX: 0 }}
        animate={{ scaleX: 1 }}
        transition={{ duration: 0.6, ease: "easeOut" }}
        style={{ transformOrigin: "left" }}
      />
      {/* Sparkle pulse */}
      <motion.span
        key={`spark-${index}`}
        className="absolute -top-2 -right-2 h-2 w-2 rounded-full bg-cyan-400"
        initial={{ opacity: 0, scale: 0 }}
        animate={{ opacity: [0,1,0], scale: [0,1,0] }}
        transition={{ duration: 0.8, ease: "easeOut" }}
      />
    </span>
  );
}

function PortfolioContent() {
  const { isDark, setIsDark } = useThemeToggle();
  const [activeId, setActiveId] = useState("about");

  // Smooth anchor navigation
  const handleNav = (e, id) => {
    if (e) e.preventDefault();
    const el = document.getElementById(id);
    if (el) {
      el.scrollIntoView({ behavior: "smooth", block: "start" });
      try { history.replaceState(null, "", `#${id}`); } catch {}
    }
  };

  // Observe sections to highlight active nav item
  useEffect(() => {
    const ids = ["about", "experience", "projects", "skills", "why", "education", "playbook", "contact"]; 
    const sections = ids
      .map((id) => document.getElementById(id))
      .filter(Boolean);

    if (!sections.length) return;

    const io = new IntersectionObserver(
      (entries) => {
        const visible = entries
          .filter((e) => e.isIntersecting)
          .sort((a, b) => b.intersectionRatio - a.intersectionRatio)[0];
        if (visible?.target?.id) setActiveId(visible.target.id);
      },
      { root: null, threshold: [0.4, 0.6], rootMargin: "-64px 0px -40% 0px" }
    );

    sections.forEach((s) => io.observe(s));
    return () => io.disconnect();
  }, []);

  // Gradient blob motion seed
  const blob = useMemo(() => ({
    animate: {
      x: [0, 20, -10, 10, 0],
      y: [0, -10, 20, -15, 0],
      scale: [1, 1.05, 0.98, 1.03, 1],
      transition: { duration: 12, repeat: Infinity, ease: "easeInOut" },
    },
  }), []);

  // Exit‑intent modal state
  const [showExit, setShowExit] = useState(false);
  useEffect(() => {
    const key = 'exit-intent-seen';
    if (localStorage.getItem(key)) return;
    const onMove = (e) => { if (e.clientY <= 4) { setShowExit(true); localStorage.setItem(key, '1'); window.removeEventListener('mousemove', onMove); } };
    window.addEventListener('mousemove', onMove);
    return () => window.removeEventListener('mousemove', onMove);
  }, []);

  return (
    <div className="relative min-h-screen bg-white text-zinc-900 scroll-smooth">
      {/* Decorative gradient background */}
      <div className="pointer-events-none absolute inset-0 -z-10 overflow-hidden">
        <motion.div
          {...blob}
          className="absolute left-[-10%] top-[-10%] h-64 w-64 rounded-full bg-indigo-500/20 blur-3xl"
        />
        <motion.div
          {...blob}
          className="absolute bottom-[-10%] right-[-10%] h-72 w-72 rounded-full bg-purple-500/20 blur-3xl"
        />
      </div>

      {/* Grain texture + Neural grid + Cursor glow */}
      <BackgroundGrain />
      <NeuralGridBackground />
      <CursorGlow />

      {/* Header */}
      <header className="sticky top-0 z-30 border-b bg-white/60 backdrop-blur supports-[backdrop-filter]:bg-white/50">
        <ScrollProgressBar />
        <div className="mx-auto flex max-w-6xl items-center justify-between px-6 py-3">
          {/* Left: Avatar + Name */}
          <div className="flex items-center gap-3">
            <motion.div
              animate={{ scale: [1, 1.08, 1] }}
              transition={{ duration: 2, repeat: Infinity, ease: "easeInOut" }}
              className="h-9 w-9 rounded-full"
            >
              <Avatar className="h-full w-full bg-gradient-to-br from-indigo-500 via-cyan-400 to-purple-500 shadow-lg shadow-indigo-200/40 flex items-center justify-center">
                {/* Abstract AI graphic circle */}
                <div className="h-5 w-5 rounded-full bg-white/90 backdrop-blur-sm flex items-center justify-center">
                  <Sparkles className="h-3 w-3 text-indigo-600" />
                </div>
              </Avatar>
            </motion.div>
            <div className="leading-tight">
              <div className="text-base font-semibold tracking-tight">{DATA.name}</div>
              <div className="mt-0.5 text-xs text-zinc-600">{DATA.title}</div>
            </div>
          </div>

          {/* Center: Nav */}
          <nav className="hidden gap-6 md:flex">
            <NavLink href="#about" active={activeId === "about"} onClick={(e) => handleNav(e, "about")}>About me</NavLink>
            <NavLink href="#experience" active={activeId === "experience"} onClick={(e) => handleNav(e, "experience")}>Experience</NavLink>
            <NavLink href="#projects" active={activeId === "projects"} onClick={(e) => handleNav(e, "projects")}>Projects</NavLink>
            <NavLink href="#skills" active={activeId === "skills"} onClick={(e) => handleNav(e, "skills")}>Skills</NavLink>
            <NavLink href="#why" active={activeId === "why"} onClick={(e) => handleNav(e, "why")}>Why Me</NavLink>
            <NavLink href="#education" active={activeId === "education"} onClick={(e) => handleNav(e, "education")}>Education</NavLink>
            <NavLink href="#playbook" active={activeId === "playbook"} onClick={(e) => handleNav(e, "playbook")}>Playbook</NavLink>
            <NavLink href="#contact" active={activeId === "contact"} onClick={(e) => handleNav(e, "contact")}>Contact</NavLink>
          </nav>

          {/* Right: Actions */}
          <div className="flex items-center gap-2">
            <a href={DATA.resumeUrl} target="_blank" rel="noreferrer">
              <Button size="sm" className="gap-2">
                <FileDown className="h-4 w-4" /> Resume
              </Button>
            </a>
          </div>
        </div>
      </header>

      {/* Hero */}
      <section className="relative">
        <div className="mx-auto grid max-w-6xl grid-cols-1 items-center gap-8 px-6 py-14 lg:grid-cols-[1.1fr_.9fr] lg:py-20">
          <div>
            <motion.h1
              initial={{ opacity: 0, y: 12 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.45 }}
              className="text-3xl font-bold tracking-tight sm:text-4xl lg:text-5xl"
            >
              I turn <AnimatedWords words={["ambitious engineers", "senior technologists", "future leaders"]} /> into AI‑ready talent that drives business results.
            </motion.h1>
            <motion.p
              initial={{ opacity: 0, y: 12 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.5, delay: 0.05 }}
              className="mt-4 max-w-2xl text-base leading-relaxed text-muted-foreground"
            >
              {DATA.summary}
            </motion.p>
            <div className="mt-6 flex flex-wrap gap-3">
              <a href="#contact">
                <Button className="gap-2">
                  <Mail className="h-4 w-4" /> Work with me <ArrowRight className="h-4 w-4" />
                </Button>
              </a>
              <a href={DATA.linkedin} target="_blank" rel="noreferrer">
                <Button variant="outline" className="gap-2">
                  <Linkedin className="h-4 w-4" /> LinkedIn
                </Button>
              </a>
            </div>
          </div>
          <div className="grid grid-cols-2 gap-4 sm:grid-cols-4 lg:grid-cols-2">
            {DATA.highlights.map((m, idx) => (
              <TiltCard key={idx}>
                <Metric icon={m.icon} value={m.value} label={m.label} />
              </TiltCard>
            ))}
          </div>
        </div>
      </section>

      {/* About */}
      <Section id="about" title="About me">
        <div className="grid gap-6 lg:grid-cols-3">
          <Card className="lg:col-span-2 border border-border/60">
            <CardContent className="p-6 text-[15px] leading-relaxed">
              <p>
                Lokesh partners with senior engineering leaders to architect credible AI upskilling pathways, align GTM with capability building, and convert intent into measurable outcomes. The operating style balances empathy with accountability, and focuses on playbooks that scale: consultative discovery, qualification clarity, and crisp value communication.
              </p>
              <p className="mt-4">
                Recent focus areas include enterprise AI readiness, L&D program design for mid‑senior cohorts, and building repeatable enablement that compounds revenue while safeguarding learner outcomes.
              </p>
            </CardContent>
          </Card>
          <Card>
            <CardHeader>
              <CardTitle className="text-base">Reach out to me</CardTitle>
            </CardHeader>
            <CardContent className="space-y-3 text-sm">
              <div className="flex items-center gap-2 text-muted-foreground"><MapPin className="h-4 w-4" /> {DATA.location}</div>
              <a href={`mailto:${DATA.email}`} className="flex items-center gap-2 hover:underline"><Mail className="h-4 w-4" /> {DATA.email}</a>
              <a href={`tel:${DATA.phone.replace(/\s/g, "")}`} className="flex items-center gap-2 hover:underline"><Phone className="h-4 w-4" /> {DATA.phone}</a>
              <a href={DATA.linkedin} target="_blank" rel="noreferrer" className="flex items-center gap-2 hover:underline"><Linkedin className="h-4 w-4" /> {DATA.linkedin}</a>
            </CardContent>
          </Card>
        </div>
      </Section>

      {/* Experience */}
      <Section id="experience" title="Experience" accent>
        <div className="relative before:absolute before:left-4 before:top-0 before:h-full before:w-px before:bg-border sm:before:left-6">
          <div className="space-y-2 pl-2 sm:pl-6">
            {DATA.experience.map((e, i) => (
              <TimelineItem key={i} {...e} />
            ))}
          </div>
        </div>
      </Section>

      {/* Projects */}
      <Section id="projects" title="Projects">
        <div className="grid gap-6 md:grid-cols-2">
          {DATA.projects.map((p, i) => (
            <TiltCard key={i}>
              <ProjectCard {...p} />
            </TiltCard>
          ))}
        </div>
      </Section>

      {/* Skills */}
      <Section id="skills" title="Skills" accent>
        <div className="grid gap-6 md:grid-cols-2">
          {DATA.skills.map((group, i) => (
            <SkillPills key={i} {...group} />
          ))}
        </div>
      </Section>

      {/* Why Me */}
      <Section id="why" title="Why me?">
        <div className="grid gap-6 md:grid-cols-2">
          {[
            {
              title: "Outcome‑obsessed",
              desc: "Measured by uplift, efficiency, and transitions; not just activity.",
              kpi: "85% uplift · 96% efficiency · 198 transitions",
            },
            {
              title: "Enterprise credibility",
              desc: "Advised 200+ senior professionals; co‑built programs with IIT Roorkee.",
              kpi: "$50M+ revenue influence",
            },
            {
              title: "Systems builder",
              desc: "Playbooks, CRM accountability, and enablement that compound over time.",
              kpi: "50+ enablement sessions · Top 3 performer",
            },
            {
              title: "High‑integrity advisory",
              desc: "Selective intake, honest expectations, and long‑term talent outcomes.",
              kpi: "Awards: Retention Star, Rockstar, Glory",
            },
          ].map((it, i) => (
            <motion.div
              key={i}
              initial={{ opacity: 0, y: 10 }}
              whileInView={{ opacity: 1, y: 0 }}
              viewport={{ once: true }}
              transition={{ duration: 0.35 }}
            >
              <Card className="group transition-transform duration-300 hover:-translate-y-1 bg-white/70 backdrop-blur border border-transparent hover:border-indigo-400/60 hover:shadow-xl hover:bg-gradient-to-r from-indigo-50 via-cyan-50 to-purple-50">
                <CardHeader>
                  <CardTitle className="text-lg font-semibold flex items-center gap-2">
                    <Sparkles className="h-5 w-5" /> {it.title}
                  </CardTitle>
                </CardHeader>
                <CardContent className="text-sm text-muted-foreground">
                  <p>{it.desc}</p>
                  <div className="mt-3 inline-flex items-center gap-2 rounded-full bg-indigo-500/10 px-3 py-1 text-xs font-medium text-indigo-600">
                    <Star className="h-3.5 w-3.5" /> {it.kpi}
                  </div>
                </CardContent>
              </Card>
            </motion.div>
          ))}
        </div>
      </Section>

      {/* Education */}
      <Section id="education" title="Education">
        <div className="grid gap-6 md:grid-cols-3">
          {DATA.education.map((ed, i) => (
            <Card key={i} className="border border-border/60">
              <CardHeader>
                <CardTitle className="text-base font-semibold">{ed.school}</CardTitle>
              </CardHeader>
              <CardContent className="text-sm text-muted-foreground">
                <div className="mb-1">{ed.credential}</div>
                <div className="mb-1">{ed.place}</div>
                <div className="flex items-center gap-2 text-xs"><Calendar className="h-3.5 w-3.5" /> {ed.period}</div>
              </CardContent>
            </Card>
          ))}
        </div>
      </Section>

      {/* Free Playbook CTA */}
      <Section id="playbook" title="Free Playbook: AI‑Readiness Roadmap (2025)" accent>
        <Card>
          <CardContent className="flex flex-col items-center gap-4 p-8 text-center">
            <p className="max-w-3xl text-sm text-muted-foreground">
              Get my concise, battle‑tested roadmap used with 200+ senior engineers. No fluff — clear steps, role pathways, and measurement templates.
            </p>
            <div className="flex w-full max-w-xl flex-col gap-3 sm:flex-row">
              <input type="email" placeholder="Your email"
                className="w-full rounded-xl border border-zinc-300 bg-white/70 px-4 py-3 text-sm outline-none ring-0 focus:border-zinc-400" />
              <Button className="gap-2"><Mail className="h-4 w-4" /> Send me the playbook</Button>
            </div>
            <div className="text-[12px] text-muted-foreground">I’ll never spam. One high‑value resource, that’s it.</div>
          </CardContent>
        </Card>
      </Section>

      {/* Contact */}
      <Section id="contact" title="Contact" accent>
        <Card>
          <CardContent className="flex flex-col items-center gap-4 p-8 text-center">
            <h3 className="text-xl font-semibold">Let’s build something impactful</h3>
            <p className="max-w-2xl text-sm text-muted-foreground">
              Open to advisory, AI‑readiness programs, and GTM alignment projects where talent capability translates into business outcomes.
            </p>
            <div className="flex flex-wrap items-center justify-center gap-3">
              <a href={`mailto:${DATA.email}`}>
                <Button className="gap-2"><Mail className="h-4 w-4" /> Email</Button>
              </a>
              <a href={`tel:${DATA.phone.replace(/\s/g, "")}`}>
                <Button variant="outline" className="gap-2"><Phone className="h-4 w-4" /> Call</Button>
              </a>
              <a href={DATA.linkedin} target="_blank" rel="noreferrer">
                <Button variant="secondary" className="gap-2"><Linkedin className="h-4 w-4" /> LinkedIn</Button>
              </a>
            </div>
          </CardContent>
        </Card>
      </Section>

      {/* Footer */}
      <footer className="border-t">
        <div className="mx-auto flex max-w-6xl items-center justify-between px-6 py-6 text-sm text-muted-foreground">
          <div>© {new Date().getFullYear()} {DATA.name}. All rights reserved.</div>
          <div className="flex items-center gap-2">
            <a className="hover:underline" href="#">Privacy</a>
            <a className="hover:underline" href="#">Terms</a>
          </div>
        </div>
      </footer>

      {/* Exit‑intent modal */}
      {showExit && (
        <div className="fixed inset-0 z-[60] flex items-center justify-center bg-black/40 p-4">
          <div className="w-full max-w-lg rounded-2xl border border-zinc-200 bg-white/90 backdrop-blur p-6 shadow-2xl">
            <div className="flex items-start justify-between gap-4">
              <div>
                <h3 className="text-xl font-semibold">Want a personalised AI roadmap?</h3>
                <p className="mt-1 text-sm text-muted-foreground">I’ll send you a short, customised checklist to become AI‑ready based on your role.</p>
              </div>
              <button className="rounded-full p-1 hover:bg-zinc-100" onClick={() => setShowExit(false)} aria-label="Close">
                <X className="h-5 w-5" />
              </button>
            </div>
            <div className="mt-4 flex w-full flex-col gap-3 sm:flex-row">
              <input type="email" placeholder="Work email"
                className="w-full rounded-xl border border-zinc-300 bg-white px-4 py-3 text-sm outline-none focus:border-zinc-400" />
              <Button className="gap-2"><Mail className="h-4 w-4" /> Send checklist</Button>
            </div>
            <div className="mt-2 text-[12px] text-muted-foreground">No spam — just the checklist. You can also <a href="#contact" className="underline">reach out directly</a>.</div>
          </div>
        </div>
      )}
    </div>
  );
}

// Simple error boundary to display a friendly message instead of a blank preview
class ErrorBoundary extends React.Component { 
  constructor(props){ super(props); this.state = { hasError: false, error: null }; }
  static getDerivedStateFromError(error){ return { hasError: true, error }; }
  componentDidCatch(error, info){ console.warn('[Portfolio ErrorBoundary]', error, info); }
  render(){
    if(this.state.hasError){
      return (
        <div style={{padding:24,fontFamily:'ui-sans-serif,system-ui',color:'#111'}}>
          <h2 style={{fontSize:18,fontWeight:700,marginBottom:8}}>Something went wrong while rendering the preview.</h2>
          <div style={{fontSize:14,opacity:.7}}>If this happened after an edit, try again. If it persists, share the error shown in the console.</div>
        </div>
      );
    }
    return this.props.children;
  }
}

export default function Portfolio(){
  return (
    <ErrorBoundary>
      <PortfolioContent />
    </ErrorBoundary>
  );
}

// --- Lightweight self-tests to prevent regressions (won't break UI) ---
(function selfTests(){
  try {
    console.assert(DATA && DATA.name, 'DATA.name must exist');
    console.assert(Array.isArray(DATA.highlights) && DATA.highlights.length >= 3, 'Need at least 3 highlight metrics');
    console.assert(typeof document !== 'undefined', 'document must exist');
  } catch (e) {
    console.warn('[Portfolio self-tests] warning:', e);
  }
})();
