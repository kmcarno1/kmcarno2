import React, { useEffect, useMemo, useState } from "react";
import { motion } from "framer-motion";
import { ArrowRight, Check, Lock, Mail, Search, Sparkles, Workflow } from "lucide-react";

// shadcn/ui
import { Button } from "@/components/ui/button";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { Badge } from "@/components/ui/badge";
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";
import { Separator } from "@/components/ui/separator";

/**
 * Purvi AI – Landing page + Waitlist
 * - Single-file React component
 * - Tailwind styling
 * - No backend: stores submissions in localStorage (demo-ready)
 */

const LS_KEY = "purvi_ai_waitlist_v1";

function cn(...classes: Array<string | false | null | undefined>) {
  return classes.filter(Boolean).join(" ");
}

type WaitlistEntry = {
  id: string;
  name: string;
  email: string;
  company?: string;
  role?: string;
  createdAt: string;
};

function readWaitlist(): WaitlistEntry[] {
  try {
    const raw = localStorage.getItem(LS_KEY);
    if (!raw) return [];
    const parsed = JSON.parse(raw);
    if (!Array.isArray(parsed)) return [];
    return parsed;
  } catch {
    return [];
  }
}

function writeWaitlist(entries: WaitlistEntry[]) {
  localStorage.setItem(LS_KEY, JSON.stringify(entries));
}

function useWaitlist() {
  const [entries, setEntries] = useState<WaitlistEntry[]>([]);

  useEffect(() => {
    setEntries(readWaitlist());
  }, []);

  const count = entries.length;

  const add = (entry: Omit<WaitlistEntry, "id" | "createdAt">) => {
    const next: WaitlistEntry = {
      id: crypto?.randomUUID?.() ?? String(Date.now()),
      createdAt: new Date().toISOString(),
      ...entry,
    };
    const updated = [next, ...entries];
    setEntries(updated);
    writeWaitlist(updated);
    return next;
  };

  const exists = (email: string) =>
    entries.some((e) => e.email.trim().toLowerCase() === email.trim().toLowerCase());

  return { entries, count, add, exists };
}

const fadeUp = {
  hidden: { opacity: 0, y: 18 },
  show: (i = 0) => ({
    opacity: 1,
    y: 0,
    transition: { duration: 0.6, ease: "easeOut", delay: i * 0.06 },
  }),
};

const glass =
  "bg-white/5 backdrop-blur-xl border border-white/10 shadow-[0_20px_80px_-30px_rgba(0,0,0,0.8)]";

export default function PurviAILanding() {
  const { count, add, exists } = useWaitlist();

  const [form, setForm] = useState({
    name: "",
    email: "",
    company: "",
    role: "",
  });
  const [status, setStatus] = useState<
    | { kind: "idle" }
    | { kind: "success"; message: string }
    | { kind: "error"; message: string }
  >({ kind: "idle" });

  const heroStats = useMemo(
    () => [
      { label: "Unified context", value: "Email + chat + docs" },
      { label: "Drafts in your voice", value: "Tone-aware" },
      { label: "Search everything", value: "Instant recall" },
    ],
    []
  );

  const onSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    setStatus({ kind: "idle" });

    const name = form.name.trim();
    const email = form.email.trim();
    const company = form.company.trim();
    const role = form.role.trim();

    if (!name) return setStatus({ kind: "error", message: "Please enter your name." });

    const emailOk = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
    if (!emailOk)
      return setStatus({ kind: "error", message: "Please enter a valid email." });

    if (exists(email)) {
      return setStatus({
        kind: "success",
        message: "You’re already on the waitlist. We’ll email you soon.",
      });
    }

    add({ name, email, company: company || undefined, role: role || undefined });
    setStatus({
      kind: "success",
      message: "Added! You’re on the waitlist. We’ll reach out with early access.",
    });
    setForm({ name: "", email: "", company: "", role: "" });
  };

  const scrollToId = (id: string) => {
    const el = document.getElementById(id);
    if (!el) return;
    el.scrollIntoView({ behavior: "smooth", block: "start" });
  };

  return (
    <div className="min-h-screen bg-[#07070A] text-white">
      {/* Background */}
      <div aria-hidden className="fixed inset-0 overflow-hidden">
        <div className="absolute -top-40 left-1/2 h-[520px] w-[520px] -translate-x-1/2 rounded-full bg-white/10 blur-[110px]" />
        <div className="absolute top-32 -left-24 h-[520px] w-[520px] rounded-full bg-white/5 blur-[120px]" />
        <div className="absolute -bottom-40 right-0 h-[620px] w-[620px] rounded-full bg-white/5 blur-[140px]" />
        <div className="absolute inset-0 bg-[radial-gradient(circle_at_1px_1px,rgba(255,255,255,0.06)_1px,transparent_0)] [background-size:28px_28px] opacity-30" />
        <div className="absolute inset-0 bg-gradient-to-b from-black/40 via-black/40 to-black" />
      </div>

      {/* Top nav */}
      <header className="relative z-10 mx-auto max-w-6xl px-6 pt-6">
        <div className={cn("flex items-center justify-between rounded-2xl px-4 py-3", glass)}>
          <button
            onClick={() => scrollToId("top")}
            className="flex items-center gap-2 rounded-xl px-2 py-1 hover:bg-white/5"
          >
            <div className="grid h-9 w-9 place-items-center rounded-xl bg-white/10">
              <Sparkles className="h-5 w-5" />
            </div>
            <div className="leading-tight">
              <div className="text-sm font-semibold tracking-wide">Purvi AI</div>
              <div className="text-xs text-white/60">Assistant for modern work</div>
            </div>
          </button>

          <nav className="hidden items-center gap-1 md:flex">
            {[
              { id: "about", label: "About" },
              { id: "features", label: "Features" },
              { id: "faqs", label: "FAQs" },
            ].map((item) => (
              <button
                key={item.id}
                onClick={() => scrollToId(item.id)}
                className="rounded-xl px-3 py-2 text-sm text-white/80 hover:bg-white/5 hover:text-white"
              >
                {item.label}
              </button>
            ))}
          </nav>

          <div className="flex items-center gap-2">
            <Button
              variant="secondary"
              className="hidden md:inline-flex bg-white/10 text-white hover:bg-white/15"
              onClick={() => scrollToId("waitlist")}
            >
              Join waitlist
              <ArrowRight className="ml-2 h-4 w-4" />
            </Button>
            <Button
              className="bg-white text-black hover:bg-white/90"
              onClick={() => scrollToId("waitlist")}
            >
              Get started
            </Button>
          </div>
        </div>
      </header>

      {/* Hero */}
      <main id="top" className="relative z-10">
        <section className="mx-auto max-w-6xl px-6 pb-16 pt-10 md:pb-24 md:pt-16">
          <div className="grid items-center gap-10 md:grid-cols-2">
            <div>
              <motion.div
                variants={fadeUp}
                initial="hidden"
                animate="show"
                custom={0}
                className="inline-flex items-center gap-2 rounded-full border border-white/10 bg-white/5 px-3 py-1 text-xs text-white/80"
              >
                <Badge className="bg-white text-black hover:bg-white">Early access</Badge>
                <span>Join {count} others on the waitlist</span>
              </motion.div>

              <motion.h1
                variants={fadeUp}
                initial="hidden"
                animate="show"
                custom={1}
                className="mt-5 text-4xl font-semibold leading-[1.05] tracking-tight md:text-6xl"
              >
                One assistant,
                <span className="block text-white/70">every workflow.</span>
              </motion.h1>

              <motion.p
                variants={fadeUp}
                initial="hidden"
                animate="show"
                custom={2}
                className="mt-5 max-w-xl text-base leading-relaxed text-white/70 md:text-lg"
              >
                Purvi AI brings your conversations, notes, and tasks into one place. It learns what matters,
                drafts replies in your voice, and helps you move faster—without losing the human touch.
              </motion.p>

              <motion.div
                variants={fadeUp}
                initial="hidden"
                animate="show"
                custom={3}
                className="mt-7 flex flex-col gap-3 sm:flex-row"
              >
                <Button
                  size="lg"
                  className="bg-white text-black hover:bg-white/90"
                  onClick={() => scrollToId("waitlist")}
                >
                  Join the waitlist
                  <ArrowRight className="ml-2 h-4 w-4" />
                </Button>
                <Button
                  size="lg"
                  variant="secondary"
                  className="bg-white/10 text-white hover:bg-white/15"
                  onClick={() => scrollToId("features")}
                >
                  See features
                </Button>
              </motion.div>

              <motion.div
                variants={fadeUp}
                initial="hidden"
                animate="show"
                custom={4}
                className="mt-8 grid max-w-xl grid-cols-1 gap-3 sm:grid-cols-3"
              >
                {heroStats.map((s) => (
                  <div
                    key={s.label}
                    className="rounded-2xl border border-white/10 bg-white/5 px-4 py-3"
                  >
                    <div className="text-xs text-white/60">{s.label}</div>
                    <div className="mt-1 text-sm font-medium text-white/90">{s.value}</div>
                  </div>
                ))}
              </motion.div>
            </div>

            {/* Product mock */}
            <motion.div
              initial={{ opacity: 0, y: 18 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.7, ease: "easeOut" }}
              className={cn("relative overflow-hidden rounded-3xl", glass)}
            >
              <div className="absolute inset-0 bg-gradient-to-br from-white/10 via-transparent to-transparent" />
              <div className="relative p-5 md:p-6">
                <div className="flex items-center justify-between">
                  <div className="flex items-center gap-2">
                    <div className="grid h-9 w-9 place-items-center rounded-xl bg-white/10">
                      <Workflow className="h-5 w-5" />
                    </div>
                    <div>
                      <div className="text-sm font-semibold">Unified Inbox</div>
                      <div className="text-xs text-white/60">Priority-ranked by intent</div>
                    </div>
                  </div>
                  <Badge className="bg-white/10 text-white hover:bg-white/10">Live preview</Badge>
                </div>

                <div className="mt-5 rounded-2xl border border-white/10 bg-black/40 p-4">
                  <div className="flex items-center gap-2 text-xs text-white/60">
                    <Search className="h-4 w-4" />
                    <span>Ask: “What do I need to reply to today?”</span>
                  </div>
                  <div className="mt-4 space-y-3">
                    {["Investor follow‑up", "Client approval", "Team standup action items"].map((t, i) => (
                      <div
                        key={t}
                        className="flex items-start justify-between gap-4 rounded-xl border border-white/10 bg-white/5 p-3"
                      >
                        <div>
                          <div className="text-sm font-medium">{t}</div>
                          <div className="mt-1 text-xs text-white/60">
                            Suggested reply ready • Context linked
                          </div>
                        </div>
                        <div className="mt-0.5 rounded-full bg-white/10 px-2 py-1 text-xs text-white/70">
                          P{i + 1}
                        </div>
                      </div>
                    ))}
                  </div>

                  <div className="mt-4 rounded-xl border border-white/10 bg-white/5 p-3">
                    <div className="flex items-center justify-between">
                      <div className="text-xs text-white/60">Draft reply</div>
                      <div className="inline-flex items-center gap-1 text-xs text-white/70">
                        <Lock className="h-3.5 w-3.5" />
                        Private
                      </div>
                    </div>
                    <div className="mt-2 text-sm text-white/85">
                      “Sharing the updated deck now. Happy to walk you through the key changes—what time works best?”
                    </div>
                    <div className="mt-3 flex gap-2">
                      <Button className="h-9 bg-white text-black hover:bg-white/90">Send</Button>
                      <Button variant="secondary" className="h-9 bg-white/10 text-white hover:bg-white/15">
                        Edit
                      </Button>
                    </div>
                  </div>
                </div>

                <div className="mt-5 grid grid-cols-3 gap-3">
                  {["Email", "WhatsApp", "Slack"].map((x) => (
                    <div
                      key={x}
                      className="grid place-items-center rounded-2xl border border-white/10 bg-white/5 py-4 text-sm text-white/80"
                    >
                      {x}
                    </div>
                  ))}
                </div>
              </div>
            </motion.div>
          </div>
        </section>

        {/* About */}
        <section id="about" className="mx-auto max-w-6xl px-6 pb-16">
          <div className="grid gap-6 md:grid-cols-3">
            {[{ icon: Mail, title: "Respond faster", desc: "Auto-drafted replies that match your tone—edit, approve, or ignore." },
              { icon: Search, title: "Find anything", desc: "Search across messages, docs, and notes without remembering exact words." },
              { icon: Sparkles, title: "Stay focused", desc: "Daily briefings that highlight what matters and what can wait." },
            ].map((c, i) => (
              <motion.div
                key={c.title}
                variants={fadeUp}
                initial="hidden"
                whileInView="show"
                viewport={{ once: true, amount: 0.35 }}
                custom={i}
              >
                <Card className={cn(glass, "rounded-3xl")}
                  style={{ backgroundColor: "rgba(255,255,255,0.04)" }}
                >
                  <CardHeader>
                    <div className="flex items-center gap-3">
                      <div className="grid h-10 w-10 place-items-center rounded-2xl bg-white/10">
                        <c.icon className="h-5 w-5" />
                      </div>
                      <CardTitle className="text-base">{c.title}</CardTitle>
                    </div>
                  </CardHeader>
                  <CardContent className="text-sm text-white/70">{c.desc}</CardContent>
                </Card>
              </motion.div>
            ))}
          </div>
        </section>

        {/* Integrations marquee */}
        <section className="mx-auto max-w-6xl px-6 pb-16">
          <div className={cn("rounded-3xl px-6 py-6", glass)}>
            <div className="flex items-center justify-between gap-4">
              <div>
                <div className="text-xs tracking-widest text-white/60">INTEGRATIONS</div>
                <div className="mt-1 text-lg font-semibold">Connect what you already use</div>
              </div>
              <Badge className="bg-white/10 text-white hover:bg-white/10">More soon</Badge>
            </div>

            <div className="mt-5 overflow-hidden">
              <div className="relative">
                <div className="pointer-events-none absolute inset-y-0 left-0 w-16 bg-gradient-to-r from-black/60 to-transparent" />
                <div className="pointer-events-none absolute inset-y-0 right-0 w-16 bg-gradient-to-l from-black/60 to-transparent" />

                <div className="flex gap-3 [mask-image:linear-gradient(to_right,transparent,black_10%,black_90%,transparent)]">
                  <div className="flex min-w-full animate-[scroll_22s_linear_infinite] gap-3">
                    {[
                      "Gmail",
                      "Google Calendar",
                      "Slack",
                      "WhatsApp",
                      "Notion",
                      "Linear",
                      "HubSpot",
                      "Google Drive",
                      "Zoom",
                      "LinkedIn",
                    ].map((b) => (
                      <div
                        key={b}
                        className="shrink-0 rounded-2xl border border-white/10 bg-white/5 px-4 py-3 text-sm text-white/80"
                      >
                        {b}
                      </div>
                    ))}
                  </div>
                  <div className="flex min-w-full animate-[scroll_22s_linear_infinite] gap-3" aria-hidden>
                    {[
                      "Gmail",
                      "Google Calendar",
                      "Slack",
                      "WhatsApp",
                      "Notion",
                      "Linear",
                      "HubSpot",
                      "Google Drive",
                      "Zoom",
                      "LinkedIn",
                    ].map((b) => (
                      <div
                        key={b + "_2"}
                        className="shrink-0 rounded-2xl border border-white/10 bg-white/5 px-4 py-3 text-sm text-white/80"
                      >
                        {b}
                      </div>
                    ))}
                  </div>
                </div>

                <style>{`@keyframes scroll{from{transform:translateX(0)}to{transform:translateX(-100%)}}`}</style>
              </div>
            </div>
          </div>
        </section>

        {/* Features */}
        <section id="features" className="mx-auto max-w-6xl px-6 pb-16">
          <div className="mb-6 flex items-end justify-between gap-4">
            <div>
              <div className="text-xs tracking-widest text-white/60">FEATURES</div>
              <h2 className="mt-2 text-2xl font-semibold tracking-tight md:text-3xl">
                Built for fast, high-context teams
              </h2>
            </div>
            <Button
              variant="secondary"
              className="hidden bg-white/10 text-white hover:bg-white/15 md:inline-flex"
              onClick={() => scrollToId("waitlist")}
            >
              Get early access
              <ArrowRight className="ml-2 h-4 w-4" />
            </Button>
          </div>

          <div className="grid gap-6 md:grid-cols-2">
            {[
              {
                title: "Auto-drafted replies",
                blurb:
                  "Purvi learns how you communicate with different people and prepares drafts you can send in seconds.",
                bullets: [
                  "Tone + intent aware",
                  "Uses recent context",
                  "Always editable",
                ],
              },
              {
                title: "Universal search",
                blurb:
                  "Search across all conversations and documents—even if you don’t remember the exact wording.",
                bullets: ["Cross-platform", "Fast recall", "Semantic matching"],
              },
              {
                title: "Connected context",
                blurb:
                  "Purvi links what belongs together—meeting notes, emails, and chats—before you even ask.",
                bullets: ["Smart linking", "Thread grouping", "Less tab-hopping"],
              },
              {
                title: "Daily briefing",
                blurb:
                  "Start your day with a prioritized list of what needs attention and suggested next steps.",
                bullets: ["Action items", "Priority ranking", "Focused flow"],
              },
            ].map((f, i) => (
              <motion.div
                key={f.title}
                variants={fadeUp}
                initial="hidden"
                whileInView="show"
                viewport={{ once: true, amount: 0.25 }}
                custom={i}
              >
                <Card className={cn(glass, "rounded-3xl")}
                  style={{ backgroundColor: "rgba(255,255,255,0.04)" }}
                >
                  <CardHeader>
                    <CardTitle className="text-lg">{f.title}</CardTitle>
                  </CardHeader>
                  <CardContent>
                    <p className="text-sm leading-relaxed text-white/70">{f.blurb}</p>
                    <div className="mt-4 space-y-2">
                      {f.bullets.map((b) => (
                        <div key={b} className="flex items-center gap-2 text-sm text-white/80">
                          <span className="grid h-5 w-5 place-items-center rounded-full bg-white/10">
                            <Check className="h-3.5 w-3.5" />
                          </span>
                          <span>{b}</span>
                        </div>
                      ))}
                    </div>
                  </CardContent>
                </Card>
              </motion.div>
            ))}
          </div>
        </section>

        {/* Waitlist */}
        <section id="waitlist" className="mx-auto max-w-6xl px-6 pb-16">
          <div className={cn("grid gap-6 rounded-3xl p-6 md:grid-cols-2 md:p-10", glass)}>
            <div>
              <div className="text-xs tracking-widest text-white/60">WAITLIST</div>
              <h3 className="mt-2 text-2xl font-semibold tracking-tight md:text-3xl">
                Get early access to Purvi AI
              </h3>
              <p className="mt-3 max-w-lg text-sm leading-relaxed text-white/70">
                We’re onboarding in stages to keep quality high. Join the waitlist and we’ll email you when your
                invite is ready.
              </p>

              <div className="mt-6 grid gap-3">
                {["Private by default", "No spam — only launch + access updates", "Built for founders & operators"].map(
                  (x) => (
                    <div key={x} className="flex items-center gap-2 text-sm text-white/80">
                      <span className="grid h-5 w-5 place-items-center rounded-full bg-white/10">
                        <Check className="h-3.5 w-3.5" />
                      </span>
                      <span>{x}</span>
                    </div>
                  )
                )}
              </div>

              <Separator className="my-6 bg-white/10" />

              <div className="text-sm text-white/70">
                <span className="font-medium text-white">{count}</span> people are already in line.
              </div>
            </div>

            <div className="rounded-3xl border border-white/10 bg-black/30 p-5 md:p-6">
              <form onSubmit={onSubmit} className="space-y-4">
                <div className="grid gap-2">
                  <Label htmlFor="name">Name</Label>
                  <Input
                    id="name"
                    placeholder="Your name"
                    value={form.name}
                    onChange={(e) => setForm((p) => ({ ...p, name: e.target.value }))}
                    className="border-white/10 bg-white/5 text-white placeholder:text-white/40"
                  />
                </div>

                <div className="grid gap-2">
                  <Label htmlFor="email">Email</Label>
                  <div className="relative">
                    <Mail className="pointer-events-none absolute left-3 top-1/2 h-4 w-4 -translate-y-1/2 text-white/50" />
                    <Input
                      id="email"
                      placeholder="you@company.com"
                      type="email"
                      value={form.email}
                      onChange={(e) => setForm((p) => ({ ...p, email: e.target.value }))}
                      className="pl-10 border-white/10 bg-white/5 text-white placeholder:text-white/40"
                    />
                  </div>
                </div>

                <div className="grid gap-4 md:grid-cols-2">
                  <div className="grid gap-2">
                    <Label htmlFor="company">Company (optional)</Label>
                    <Input
                      id="company"
                      placeholder="Purvi Labs"
                      value={form.company}
                      onChange={(e) => setForm((p) => ({ ...p, company: e.target.value }))}
                      className="border-white/10 bg-white/5 text-white placeholder:text-white/40"
                    />
                  </div>
                  <div className="grid gap-2">
                    <Label htmlFor="role">Role (optional)</Label>
                    <Input
                      id="role"
                      placeholder="Founder / Ops"
                      value={form.role}
                      onChange={(e) => setForm((p) => ({ ...p, role: e.target.value }))}
                      className="border-white/10 bg-white/5 text-white placeholder:text-white/40"
                    />
                  </div>
                </div>

                <Button
                  type="submit"
                  size="lg"
                  className="w-full bg-white text-black hover:bg-white/90"
                >
                  Join waitlist
                  <ArrowRight className="ml-2 h-4 w-4" />
                </Button>

                {status.kind !== "idle" && (
                  <div
                    className={cn(
                      "rounded-2xl border px-4 py-3 text-sm",
                      status.kind === "success"
                        ? "border-white/10 bg-white/5 text-white/85"
                        : "border-white/10 bg-white/5 text-white/85"
                    )}
                  >
                    <div className="flex items-start gap-2">
                      <span className="mt-0.5 grid h-5 w-5 place-items-center rounded-full bg-white/10">
                        {status.kind === "success" ? (
                          <Check className="h-3.5 w-3.5" />
                        ) : (
                          <span className="text-xs">!</span>
                        )}
                      </span>
                      <div>{status.message}</div>
                    </div>
                  </div>
                )}

                <p className="text-xs text-white/50">
                  Demo note: this form stores signups in <span className="text-white/70">localStorage</span>.
                  Hook it to your backend (Supabase/Firebase/Sheets) for production.
                </p>
              </form>
            </div>
          </div>
        </section>

        {/* FAQs */}
        <section id="faqs" className="mx-auto max-w-6xl px-6 pb-20">
          <div className="mb-6">
            <div className="text-xs tracking-widest text-white/60">FAQS</div>
            <h3 className="mt-2 text-2xl font-semibold tracking-tight md:text-3xl">Frequently asked questions</h3>
            <p className="mt-2 max-w-2xl text-sm text-white/70">
              Everything you need to know about Purvi AI—setup, privacy, and how early access works.
            </p>
          </div>

          <div className={cn("rounded-3xl p-2 md:p-4", glass)}>
            <Accordion type="single" collapsible className="w-full">
              <AccordionItem value="i1" className="border-white/10">
                <AccordionTrigger className="px-4 text-left">How is Purvi AI different from a normal inbox?</AccordionTrigger>
                <AccordionContent className="px-4 text-white/70">
                  Traditional inboxes sort by recency and live on one platform. Purvi AI unifies your channels,
                  ranks items by priority, and keeps the full context attached so you can respond faster.
                </AccordionContent>
              </AccordionItem>
              <AccordionItem value="i2" className="border-white/10">
                <AccordionTrigger className="px-4 text-left">How do auto-drafted replies work?</AccordionTrigger>
                <AccordionContent className="px-4 text-white/70">
                  Purvi uses your recent thread context and communication patterns to propose a draft. You can edit,
                  approve, or ignore—Purvi is a copilot, not an autopilot.
                </AccordionContent>
              </AccordionItem>
              <AccordionItem value="i3" className="border-white/10">
                <AccordionTrigger className="px-4 text-left">Is my data private and secure?</AccordionTrigger>
                <AccordionContent className="px-4 text-white/70">
                  Privacy is a first-class feature. Purvi is designed to minimize exposure: access controls,
                  encryption in transit and at rest (when connected to a backend), and no selling your data.
                </AccordionContent>
              </AccordionItem>
              <AccordionItem value="i4" className="border-white/10">
                <AccordionTrigger className="px-4 text-left">What happens after I join the waitlist?</AccordionTrigger>
                <AccordionContent className="px-4 text-white/70">
                  You’ll receive an email when your invite is ready. We onboard in stages to maintain performance and
                  collect feedback from early users.
                </AccordionContent>
              </AccordionItem>
            </Accordion>
          </div>
        </section>

        {/* Footer */}
        <footer className="relative z-10 border-t border-white/10">
          <div className="mx-auto max-w-6xl px-6 py-10">
            <div className="flex flex-col justify-between gap-6 md:flex-row md:items-center">
              <div className="flex items-center gap-3">
                <div className="grid h-10 w-10 place-items-center rounded-2xl bg-white/10">
                  <Sparkles className="h-5 w-5" />
                </div>
                <div>
                  <div className="text-sm font-semibold">Purvi AI</div>
                  <div className="text-xs text-white/60">© {new Date().getFullYear()} Purvi AI. All rights reserved.</div>
                </div>
              </div>

              <div className="flex flex-wrap gap-2">
                {[
                  { label: "About", id: "about" },
                  { label: "Features", id: "features" },
                  { label: "FAQs", id: "faqs" },
                  { label: "Waitlist", id: "waitlist" },
                ].map((x) => (
                  <Button
                    key={x.id}
                    variant="secondary"
                    className="bg-white/5 text-white/80 hover:bg-white/10"
                    onClick={() => scrollToId(x.id)}
                  >
                    {x.label}
                  </Button>
                ))}
              </div>
            </div>
          </div>
        </footer>
      </main>
    </div>
  );
}
