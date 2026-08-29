# The Bet · FinWise

> Module 1 · Ignite a PLG Motion. The growth hypothesis and where you're betting, plus the growth-loop visualization.

## Growth hypothesis

_IF we redesign the first-session experience to guide new users directly to one core action (e.g., connecting a bank account or generating a first report) instead of a generic dashboard, using an endowed-progress ledger that starts pre-credited at 25%
THEN trial-to-paid conversion WILL increase BY 1-2 percentage points (from 2% to 3-4%), with day-1 core-action completion as a leading indicator
BECAUSE most trial users never reach their first "aha moment" — they land in a generic experience, don't know what action proves the product's value, and drop off before that value becomes clear; showing early, partially-completed progress exploits the goal-gradient effect to keep them moving toward it
MEASURED WITH an A/B test over 3 weeks, comparing the endowed-progress flow against the standard flow, with day-1 core-action completion as the primary metric and trial-to-paid conversion at day 14 as the downstream guardrail.

_____

## The bet

_Growth loop: Content-Driven
Reasoning: Diagnose with data first Before writing any hypothesis, pull the relevant funnel/cohort data: for activation, step-by-step drop-off in onboarding; for retention, the churned-vs-retained segmentation we discussed. Don't skip this — it's the difference between testing an informed guess and testing a hunch. Alternate between activation and retention.
Formalized: FinWise's biggest growth problem is Only 2% of trial users convert to paying customers. because Most trial users never reach their first core "aha moment" — they land in a generic experience, don't know what action proves the product's value, and drop off before that value becomes clear., and the highest-leverage experiment I'd run first is Redesign the first-session experience to guide new users directly to one core action (e.g., connecting a bank account or generating a first report) instead of a generic dashboard, and measure whether trial-to-paid conversion improves over a 3-week A/B test, with day-1 core-action completion as the primary metric.

_____

## Growth loop

import React, { useMemo } from "react";
import {
  LineChart,
  Line,
  XAxis,
  YAxis,
  Tooltip,
  Legend,
  ResponsiveContainer,
  CartesianGrid,
} from "recharts";

// ---- Design tokens (matches prior FinWise artifacts) -------------------
const colors = {
  ink: "#1B2B26",
  inkSoft: "#4A5750",
  paper: "#F6F3EC",
  paperCard: "#FFFFFF",
  rule: "#D8D2C2",
  green: "#2F6F52",
  gold: "#B8863F",
  goldSoft: "#F3E7D3",
  red: "#A8452F",
};
const displayFont = "'Fraunces', serif";
const bodyFont = "'IBM Plex Sans', sans-serif";
const monoFont = "'IBM Plex Mono', monospace";
const FONT_IMPORT = `
@import url('https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600&family=IBM+Plex+Sans:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500&display=swap');
`;

// ---- Raw dataset, chronological (oldest -> newest) ----------------------
const raw = [
  { month: "2023-10", visits: 7433, trials: 570, paid: 11, churned: 13, importPct: 40.17, modelingPct: 31.17, avgSession: 5.95, sessionsPerUser: 9, revenue: 859375 },
  { month: "2023-11", visits: 7391, trials: 487, paid: 10, churned: 26, importPct: 36.29, modelingPct: 10.78, avgSession: 9.49, sessionsPerUser: 7, revenue: 781250 },
  { month: "2023-12", visits: 5769, trials: 469, paid: 9, churned: 23, importPct: 42.73, modelingPct: 18.26, avgSession: 13.5, sessionsPerUser: 5, revenue: 703125 },
  { month: "2024-01", visits: 6685, trials: 358, paid: 7, churned: 12, importPct: 30.63, modelingPct: 24.81, avgSession: 14.15, sessionsPerUser: 3, revenue: 546875 },
  { month: "2024-02", visits: 5130, trials: 348, paid: 7, churned: 24, importPct: 32.16, modelingPct: 23.8, avgSession: 11.51, sessionsPerUser: 5, revenue: 546875 },
  { month: "2024-03", visits: 7919, trials: 644, paid: 13, churned: 17, importPct: 30.51, modelingPct: 40.35, avgSession: 14.26, sessionsPerUser: 5, revenue: 1015625 },
  { month: "2024-04", visits: 8171, trials: 535, paid: 11, churned: 38, importPct: 38.55, modelingPct: 58.26, avgSession: 11.06, sessionsPerUser: 8, revenue: 859375 },
  { month: "2024-05", visits: 8444, trials: 552, paid: 11, churned: 21, importPct: 40.45, modelingPct: 39.34, avgSession: 12.9, sessionsPerUser: 6, revenue: 859375 },
  { month: "2024-06", visits: 9426, trials: 321, paid: 6, churned: 16, importPct: 39.88, modelingPct: 36.98, avgSession: 12.11, sessionsPerUser: 5, revenue: 468750 },
  { month: "2024-07", visits: 5466, trials: 613, paid: 12, churned: 37, importPct: 45.42, modelingPct: 47.37, avgSession: 6.99, sessionsPerUser: 7, revenue: 937500 },
  { month: "2024-08", visits: 8092, trials: 460, paid: 9, churned: 24, importPct: 41.23, modelingPct: 52.23, avgSession: 5.14, sessionsPerUser: 8, revenue: 703125 },
  { month: "2024-09", visits: 8772, trials: 576, paid: 12, churned: 37, importPct: 45.22, modelingPct: 58.06, avgSession: 7.93, sessionsPerUser: 9, revenue: 937500 },
  { month: "2024-10", visits: 5860, trials: 491, paid: 10, churned: 29, importPct: 44.26, modelingPct: 36.04, avgSession: 8.95, sessionsPerUser: 9, revenue: 781250 },
];

function Kpi({ label, value, sub, tone }) {
  return (
    <div className="p-4" style={{ background: colors.paperCard, border: `1px solid ${colors.rule}` }}>
      <div style={{ fontFamily: bodyFont, fontSize: "0.68rem", color: colors.inkSoft, textTransform: "uppercase", letterSpacing: "0.08em" }}>
        {label}
      </div>
      <div style={{ fontFamily: monoFont, fontSize: "1.35rem", color: tone || colors.ink, marginTop: 6 }}>
        {value}
      </div>
      {sub && (
        <div style={{ fontFamily: bodyFont, fontSize: "0.72rem", color: colors.inkSoft, marginTop: 2 }}>
          {sub}
        </div>
      )}
    </div>
  );
}

function FunnelBar({ label, value, pctOfTop, pctOfPrev, widthPct, isFirst }) {
  return (
    <div className="mb-3">
      <div className="flex justify-between items-baseline mb-1">
        <span style={{ fontFamily: bodyFont, fontSize: "0.82rem", color: colors.ink, fontWeight: 600 }}>{label}</span>
        <span style={{ fontFamily: monoFont, fontSize: "0.85rem", color: colors.ink }}>
          {value.toLocaleString()}
        </span>
      </div>
      <div style={{ background: colors.rule, height: 28, width: "100%" }}>
        <div style={{ background: colors.green, height: 28, width: `${widthPct}%`, opacity: 0.85 }} />
      </div>
      {!isFirst && (
        <div style={{ fontFamily: monoFont, fontSize: "0.72rem", color: colors.inkSoft, marginTop: 3 }}>
          {pctOfPrev}% of previous stage · {pctOfTop}% of total visits
        </div>
      )}
    </div>
  );
}

export default function FunnelDashboard() {
  const stats = useMemo(() => {
    const totalVisits = raw.reduce((s, r) => s + r.visits, 0);
    const totalTrials = raw.reduce((s, r) => s + r.trials, 0);
    const totalPaid = raw.reduce((s, r) => s + r.paid, 0);
    const totalChurned = raw.reduce((s, r) => s + r.churned, 0);

    const withRates = raw.map((r) => ({
      ...r,
      visitToTrial: (r.trials / r.visits) * 100,
      trialToPaid: (r.paid / r.trials) * 100,
    }));

    const avgVisitToTrial = withRates.reduce((s, r) => s + r.visitToTrial, 0) / withRates.length;
    const avgTrialToPaid = withRates.reduce((s, r) => s + r.trialToPaid, 0) / withRates.length;

    const bestTrialToPaid = [...withRates].sort((a, b) => b.trialToPaid - a.trialToPaid)[0];
    const worstTrialToPaid = [...withRates].sort((a, b) => a.trialToPaid - b.trialToPaid)[0];
    const bestVisitToTrial = [...withRates].sort((a, b) => b.visitToTrial - a.visitToTrial)[0];
    const worstVisitToTrial = [...withRates].sort((a, b) => a.visitToTrial - b.visitToTrial)[0];

    return {
      totalVisits, totalTrials, totalPaid, totalChurned,
      avgVisitToTrial, avgTrialToPaid,
      bestTrialToPaid, worstTrialToPaid, bestVisitToTrial, worstVisitToTrial,
      withRates,
      overallVisitToPaid: (totalPaid / totalVisits) * 100,
    };
  }, []);

  const s = stats;

  return (
    <div className="w-full min-h-[900px] p-6" style={{ background: colors.paper, fontFamily: bodyFont }}>
      <style>{FONT_IMPORT}</style>

      <div className="mb-1" style={{ fontFamily: bodyFont, fontSize: "0.72rem", color: colors.green, textTransform: "uppercase", letterSpacing: "0.12em" }}>
        FinWise · 13-month funnel data (Oct 2023 – Oct 2024)
      </div>
      <h1 className="mb-5" style={{ fontFamily: displayFont, color: colors.ink, fontSize: "1.7rem", fontWeight: 500 }}>
        Trial Funnel Dashboard
      </h1>

      {/* KPI row */}
      <div className="grid grid-cols-2 md:grid-cols-4 gap-3 mb-6">
        <Kpi label="Avg Visit → Trial Rate" value={`${s.avgVisitToTrial.toFixed(2)}%`} />
        <Kpi label="Avg Trial → Paid Rate" value={`${s.avgTrialToPaid.toFixed(2)}%`} />
        <Kpi label="Best Month (Trial→Paid)" value={`${s.bestTrialToPaid.month}`} sub={`${s.bestTrialToPaid.trialToPaid.toFixed(2)}%`} tone={colors.green} />
        <Kpi label="Worst Month (Trial→Paid)" value={`${s.worstTrialToPaid.month}`} sub={`${s.worstTrialToPaid.trialToPaid.toFixed(2)}%`} tone={colors.red} />
        <Kpi label="Best Month (Visit→Trial)" value={`${s.bestVisitToTrial.month}`} sub={`${s.bestVisitToTrial.visitToTrial.toFixed(2)}%`} tone={colors.green} />
        <Kpi label="Worst Month (Visit→Trial)" value={`${s.worstVisitToTrial.month}`} sub={`${s.worstVisitToTrial.visitToTrial.toFixed(2)}%`} tone={colors.red} />
        <Kpi label="Total Trials (13mo)" value={s.totalTrials.toLocaleString()} />
        <Kpi label="Total Paid (13mo)" value={s.totalPaid.toLocaleString()} />
      </div>

      {/* Funnel */}
      <div className="p-5 mb-6" style={{ background: colors.paperCard, border: `1px solid ${colors.rule}` }}>
        <div style={{ fontFamily: bodyFont, fontSize: "0.78rem", color: colors.inkSoft, marginBottom: 14, textTransform: "uppercase", letterSpacing: "0.06em" }}>
          Cumulative funnel — 13 months combined
        </div>
        <FunnelBar label="Website Visits" value={s.totalVisits} widthPct={100} isFirst />
        <FunnelBar
          label="Trials Started"
          value={s.totalTrials}
          widthPct={Math.max((s.totalTrials / s.totalVisits) * 100 * 6, 8)}
          pctOfPrev={((s.totalTrials / s.totalVisits) * 100).toFixed(2)}
          pctOfTop={((s.totalTrials / s.totalVisits) * 100).toFixed(2)}
        />
        <FunnelBar
          label="Paid Conversions"
          value={s.totalPaid}
          widthPct={Math.max((s.totalPaid / s.totalVisits) * 100 * 40, 4)}
          pctOfPrev={((s.totalPaid / s.totalTrials) * 100).toFixed(2)}
          pctOfTop={s.overallVisitToPaid.toFixed(3)}
        />
        <div style={{ fontFamily: bodyFont, fontSize: "0.72rem", color: colors.inkSoft, marginTop: 8 }}>
          Bar widths are visually scaled (not linear) to keep all three stages readable given the size difference between them. Labeled percentages are exact.
        </div>
      </div>

      {/* Monthly trend */}
      <div className="p-5 mb-6" style={{ background: colors.paperCard, border: `1px solid ${colors.rule}` }}>
        <div style={{ fontFamily: bodyFont, fontSize: "0.78rem", color: colors.inkSoft, marginBottom: 10, textTransform: "uppercase", letterSpacing: "0.06em" }}>
          Monthly conversion rates
        </div>
        <ResponsiveContainer width="100%" height={260}>
          <LineChart data={s.withRates} margin={{ top: 5, right: 20, left: 0, bottom: 0 }}>
            <CartesianGrid stroke={colors.rule} strokeDasharray="2 3" vertical={false} />
            <XAxis dataKey="month" tick={{ fontFamily: bodyFont, fontSize: 9, fill: colors.inkSoft }} axisLine={{ stroke: colors.rule }} tickLine={false} />
            <YAxis tick={{ fontFamily: bodyFont, fontSize: 10, fill: colors.inkSoft }} axisLine={false} tickLine={false} unit="%" />
            <Tooltip
              contentStyle={{ fontFamily: bodyFont, fontSize: "0.75rem", border: `1px solid ${colors.rule}` }}
              formatter={(v, n) => [`${v.toFixed(2)}%`, n]}
            />
            <Legend wrapperStyle={{ fontFamily: bodyFont, fontSize: "0.78rem" }} />
            <Line type="monotone" dataKey="visitToTrial" name="Visit → Trial %" stroke={colors.gold} strokeWidth={2} dot={{ r: 2 }} />
            <Line type="monotone" dataKey="trialToPaid" name="Trial → Paid %" stroke={colors.red} strokeWidth={2} dot={{ r: 2 }} />
          </LineChart>
        </ResponsiveContainer>
      </div>

      {/* Patterns - factual only */}
      <div className="p-5 mb-6" style={{ background: colors.paperCard, border: `1px solid ${colors.rule}` }}>
        <div style={{ fontFamily: displayFont, fontSize: "1.1rem", color: colors.ink, marginBottom: 12, fontWeight: 500 }}>
          Three notable patterns in the data
        </div>
        <ol style={{ fontFamily: bodyFont, fontSize: "0.86rem", color: colors.ink, lineHeight: 1.7, paddingLeft: 18 }}>
          <li className="mb-3">
            <strong>June 2024 is the funnel's low point on multiple stages simultaneously.</strong> It has the highest Website Visits of any month (9,426) but the lowest Trials Started (321), the lowest Visit→Trial rate (3.40%, versus a 13-month average of 7.00%), and the lowest Paid count (6) in the dataset.
          </li>
          <li className="mb-3">
            <strong>Revenue is an exact linear function of Paid Conversions.</strong> Every month in the dataset equals Paid × $78,125 precisely, with zero variation (e.g., 2024-03: 13 × $78,125 = $1,015,625; 2024-06: 6 × $78,125 = $468,750). Revenue carries no information beyond what the Paid column already shows.
          </li>
          <li className="mb-3">
            <strong>Financial Modeling usage % has the widest range of any percentage metric and does not move in one direction.</strong> It ranges from 10.78% (Nov 2023) to 58.26% (Apr 2024), rises overall from Nov 2023 to a Sep 2024 peak of 58.06%, then drops to 36.04% in Oct 2024 — a rise across most of the period followed by a decline in the final month, not a steady trend in either direction.
          </li>
        </ol>
      </div>

      {/* Biggest drop-off - factual only */}
      <div className="p-5" style={{ background: colors.goldSoft, border: `1px solid ${colors.gold}40` }}>
        <div style={{ fontFamily: displayFont, fontSize: "1.05rem", color: colors.ink, marginBottom: 8, fontWeight: 500 }}>
          Biggest drop-off point
        </div>
        <p style={{ fontFamily: bodyFont, fontSize: "0.86rem", color: colors.ink, lineHeight: 1.6 }}>
          The Trials → Paid stage has the larger relative drop-off of the two funnel transitions. Across the 13 months, Visits → Trials retains an average of {s.avgVisitToTrial.toFixed(2)}% (a {(100 - s.avgVisitToTrial).toFixed(2)}% drop), while Trials → Paid retains an average of only {s.avgTrialToPaid.toFixed(2)}% (a {(100 - s.avgTrialToPaid).toFixed(2)}% drop). In absolute terms, {(s.totalTrials - s.totalPaid).toLocaleString()} of the {s.totalTrials.toLocaleString()} trial starts in this dataset did not convert to paid.
        </p>
      </div>
    </div>
  );
}

_____
