import React, { useState, useMemo } from 'react';
import { 
  LayoutDashboard, 
  TrendingUp, 
  PieChart, 
  Briefcase, 
  Users, 
  FileText, 
  Search, 
  Bell, 
  Settings, 
  ChevronRight, 
  ArrowUpRight, 
  Filter,
  Building2,
  DollarSign,
  Lock,
  CheckCircle,
  X,
  Clock,
  Download
} from 'lucide-react';

// --- Mock Data ---

const MOCK_STARTUPS = [
  {
    id: 1,
    name: "NeuroSync",
    industry: "HealthTech",
    stage: "Seed",
    valuation: 12000000,
    raised: 850000,
    goal: 2000000,
    minCheck: 25000,
    description: "Brain-computer interface for non-invasive stroke rehabilitation.",
    tags: ["AI", "MedTech", "B2B"],
    location: "Boston, MA",
    revenue: "$50k MRR",
    team: [
      { name: "Dr. Elena R.", role: "CEO", ex: "MIT" },
      { name: "James T.", role: "CTO", ex: "Neuralink" }
    ]
  },
  {
    id: 2,
    name: "GreenGrid",
    industry: "CleanTech",
    stage: "Series A",
    valuation: 45000000,
    raised: 3200000,
    goal: 5000000,
    minCheck: 50000,
    description: "Decentralized energy trading platform for residential solar owners.",
    tags: ["Energy", "Blockchain", "SaaS"],
    location: "Austin, TX",
    revenue: "$120k MRR",
    team: [
      { name: "Sarah L.", role: "CEO", ex: "Tesla" },
      { name: "Mike B.", role: "CFO", ex: "Goldman Sachs" }
    ]
  },
  {
    id: 3,
    name: "LogiDrone",
    industry: "Logistics",
    stage: "Pre-Seed",
    valuation: 6000000,
    raised: 150000,
    goal: 500000,
    minCheck: 10000,
    description: "Last-mile autonomous drone delivery specifically for pharmacies.",
    tags: ["Hardware", "Automation"],
    location: "San Francisco, CA",
    revenue: "Pre-Revenue",
    team: [
      { name: "David K.", role: "CEO", ex: "Amazon Prime Air" }
    ]
  },
  {
    id: 4,
    name: "FinGuard",
    industry: "FinTech",
    stage: "Seed",
    valuation: 15000000,
    raised: 1200000,
    goal: 2500000,
    minCheck: 25000,
    description: "AI-driven fraud detection API for neo-banks in emerging markets.",
    tags: ["Cybersecurity", "B2B", "API"],
    location: "New York, NY",
    revenue: "$85k MRR",
    team: [
      { name: "Amanda C.", role: "CEO", ex: "Stripe" }
    ]
  },
  {
    id: 5,
    name: "AgriSense",
    industry: "AgTech",
    stage: "Series A",
    valuation: 28000000,
    raised: 4100000,
    goal: 6000000,
    minCheck: 50000,
    description: "IoT sensors and satellite analytics for crop yield optimization.",
    tags: ["IoT", "SaaS", "Sustainability"],
    location: "Chicago, IL",
    revenue: "$200k MRR",
    team: [
      { name: "Robert F.", role: "CEO", ex: "John Deere" }
    ]
  }
];

// Initial pipeline state for the investor view
const INITIAL_PIPELINE = [
  { startupId: 1, status: "Screening", added: "2 days ago" },
  { startupId: 2, status: "Due Diligence", added: "1 week ago" },
  { startupId: 4, status: "Term Sheet", added: "3 weeks ago" }
];

// --- Helper Functions ---

const formatCurrency = (value) => {
  if (value >= 1000000) {
    return `$${(value / 1000000).toFixed(1)}M`;
  }
  if (value >= 1000) {
    return `$${(value / 1000).toFixed(0)}k`;
  }
  return `$${value}`;
};

const getProgress = (raised, goal) => Math.min(100, Math.round((raised / goal) * 100));

// --- Components ---

// 1. Sidebar
const Sidebar = ({ activeTab, setActiveTab, userType }) => {
  const investorMenu = [
    { id: 'dashboard', icon: LayoutDashboard, label: 'Dashboard' },
    { id: 'marketplace', icon: Building2, label: 'Marketplace' },
    { id: 'dealflow', icon: TrendingUp, label: 'Deal Flow' },
    { id: 'portfolio', icon: PieChart, label: 'Portfolio' },
  ];

  const startupMenu = [
    { id: 'dashboard', icon: LayoutDashboard, label: 'Overview' },
    { id: 'investors', icon: Users, label: 'Investor CRM' },
    { id: 'dataroom', icon: FileText, label: 'Data Room' },
    { id: 'settings', icon: Settings, label: 'Company Settings' },
  ];

  const menu = userType === 'investor' ? investorMenu : startupMenu;

  return (
    <div className="w-64 bg-slate-900 text-white h-screen flex flex-col fixed left-0 top-0 z-20 hidden md:flex">
      <div className="p-6 flex items-center gap-2 border-b border-slate-800">
        <div className="w-8 h-8 bg-indigo-500 rounded-lg flex items-center justify-center font-bold text-xl">V</div>
        <span className="text-xl font-semibold tracking-tight">VentureFlow</span>
      </div>
      
      <div className="flex-1 py-6 space-y-1">
        {menu.map((item) => (
          <button
            key={item.id}
            onClick={() => setActiveTab(item.id)}
            className={`w-full flex items-center gap-3 px-6 py-3 text-sm font-medium transition-colors
              ${activeTab === item.id 
                ? 'bg-slate-800 text-indigo-400 border-r-4 border-indigo-500' 
                : 'text-slate-400 hover:bg-slate-800/50 hover:text-white'}`}
          >
            <item.icon size={18} />
            {item.label}
          </button>
        ))}
      </div>

      <div className="p-4 border-t border-slate-800">
        <div className="bg-slate-800/50 rounded-xl p-4">
          <p className="text-xs text-slate-400 uppercase font-semibold mb-2">Current Persona</p>
          <div className="flex items-center gap-2 text-sm font-medium text-white">
            <div className={`w-2 h-2 rounded-full ${userType === 'investor' ? 'bg-emerald-400' : 'bg-blue-400'}`}></div>
            {userType === 'investor' ? 'Accredited Investor' : 'Company Founder'}
          </div>
        </div>
      </div>
    </div>
  );
};

// 2. Kanban / Pipeline Component (Investor View)
const DealFlowPipeline = ({ pipeline, setPipeline, onSelectStartup }) => {
  const stages = ["Screening", "Meeting", "Due Diligence", "Term Sheet", "Closed"];

  const getDealsByStage = (stage) => {
    return pipeline.filter(p => p.status === stage).map(p => {
      const startup = MOCK_STARTUPS.find(s => s.id === p.startupId);
      return { ...p, ...startup };
    });
  };

  const moveStage = (startupId, direction) => {
    setPipeline(prev => prev.map(p => {
      if (p.startupId === startupId) {
        const currentIndex = stages.indexOf(p.status);
        const nextIndex = direction === 'next' 
          ? Math.min(currentIndex + 1, stages.length - 1)
          : Math.max(currentIndex - 1, 0);
        return { ...p, status: stages[nextIndex] };
      }
      return p;
    }));
  };

  return (
    <div className="h-full overflow-x-auto pb-4">
      <div className="flex gap-4 min-w-max">
        {stages.map((stage) => (
          <div key={stage} className="w-72 flex-shrink-0">
            <div className="flex items-center justify-between mb-4 px-1">
              <h3 className="font-semibold text-slate-700 text-sm uppercase tracking-wide">{stage}</h3>
              <span className="bg-slate-200 text-slate-600 text-xs px-2 py-0.5 rounded-full font-medium">
                {getDealsByStage(stage).length}
              </span>
            </div>
            <div className="space-y-3">
              {getDealsByStage(stage).map((deal) => (
                <div key={deal.id} className="bg-white p-4 rounded-xl border border-slate-200 shadow-sm hover:shadow-md transition-shadow group">
                  <div className="flex justify-between items-start mb-2">
                    <span className="bg-indigo-50 text-indigo-700 text-xs px-2 py-1 rounded-md font-medium">
                      {deal.industry}
                    </span>
                    <button onClick={() => onSelectStartup(deal)} className="text-slate-400 hover:text-indigo-600">
                      <ArrowUpRight size={16} />
                    </button>
                  </div>
                  <h4 className="font-bold text-slate-900 mb-1">{deal.name}</h4>
                  <p className="text-xs text-slate-500 mb-3 line-clamp-2">{deal.description}</p>
                  
                  <div className="flex items-center justify-between pt-3 border-t border-slate-100">
                    <div className="text-xs text-slate-500">
                      <span className="font-semibold text-slate-700">{formatCurrency(deal.raised)}</span> raised
                    </div>
                    <div className="flex gap-1">
                       {/* Simple mock controls to move cards since DnD is complex for 1 file */}
                       <button 
                        onClick={() => moveStage(deal.id, 'prev')}
                        disabled={stage === stages[0]}
                        className="p-1 hover:bg-slate-100 rounded disabled:opacity-30"
                      >
                        <ChevronRight className="rotate-180" size={14} />
                      </button>
                      <button 
                        onClick={() => moveStage(deal.id, 'next')}
                        disabled={stage === stages[stages.length-1]}
                        className="p-1 hover:bg-slate-100 rounded disabled:opacity-30"
                      >
                        <ChevronRight size={14} />
                      </button>
                    </div>
                  </div>
                </div>
              ))}
              {getDealsByStage(stage).length === 0 && (
                <div className="h-24 border-2 border-dashed border-slate-200 rounded-xl flex items-center justify-center text-slate-400 text-xs">
                  Empty Stage
                </div>
              )}
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};

// 3. Startup Detail Modal
const StartupDetail = ({ startup, onClose, onInvest, isInPipeline }) => {
  const [activeTab, setActiveTab] = useState('overview');

  if (!startup) return null;

  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 backdrop-blur-sm p-4">
      <div className="bg-white w-full max-w-4xl max-h-[90vh] rounded-2xl shadow-2xl overflow-hidden flex flex-col">
        {/* Header */}
        <div className="h-32 bg-gradient-to-r from-indigo-600 to-blue-500 p-6 relative shrink-0">
          <button 
            onClick={onClose}
            className="absolute top-4 right-4 bg-white/20 hover:bg-white/30 text-white p-2 rounded-full transition-colors"
          >
            <X size={20} />
          </button>
          <div className="absolute -bottom-8 left-8 flex items-end gap-4">
            <div className="w-20 h-20 bg-white rounded-xl shadow-lg flex items-center justify-center text-3xl font-bold text-indigo-600 border-4 border-white">
              {startup.name[0]}
            </div>
            <div className="mb-2">
              <h2 className="text-2xl font-bold text-white">{startup.name}</h2>
              <p className="text-indigo-100 text-sm flex items-center gap-2">
                <Building2 size={14} /> {startup.location} • {startup.industry}
              </p>
            </div>
          </div>
        </div>

        {/* Controls */}
        <div className="pt-12 px-8 pb-4 border-b border-slate-200 flex justify-between items-center shrink-0">
          <div className="flex gap-6">
            {['Overview', 'Team', 'Financials', 'Data Room'].map((tab) => (
              <button
                key={tab}
                onClick={() => setActiveTab(tab.toLowerCase())}
                className={`text-sm font-medium pb-2 border-b-2 transition-colors ${
                  activeTab === tab.toLowerCase() 
                    ? 'text-indigo-600 border-indigo-600' 
                    : 'text-slate-500 border-transparent hover:text-slate-800'
                }`}
              >
                {tab}
              </button>
            ))}
          </div>
          {!isInPipeline ? (
            <button 
              onClick={() => onInvest(startup)}
              className="bg-indigo-600 hover:bg-indigo-700 text-white px-5 py-2 rounded-lg text-sm font-medium flex items-center gap-2 transition-colors"
            >
              <TrendingUp size={16} /> Add to Pipeline
            </button>
          ) : (
            <div className="flex items-center gap-2 text-emerald-600 text-sm font-medium bg-emerald-50 px-3 py-1 rounded-full border border-emerald-100">
              <CheckCircle size={14} /> Tracking in Pipeline
            </div>
          )}
        </div>

        {/* Content */}
        <div className="p-8 overflow-y-auto bg-slate-50 flex-1">
          {activeTab === 'overview' && (
            <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
              <div className="md:col-span-2 space-y-6">
                <div className="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                  <h3 className="font-bold text-slate-800 mb-4">About</h3>
                  <p className="text-slate-600 leading-relaxed">{startup.description}</p>
                  <div className="mt-4 flex flex-wrap gap-2">
                    {startup.tags.map(tag => (
                      <span key={tag} className="bg-slate-100 text-slate-600 px-3 py-1 rounded-full text-xs font-medium">
                        #{tag}
                      </span>
                    ))}
                  </div>
                </div>

                <div className="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                  <h3 className="font-bold text-slate-800 mb-4">Deal Highlights</h3>
                  <ul className="space-y-3">
                    <li className="flex items-start gap-3 text-sm text-slate-600">
                      <CheckCircle className="text-emerald-500 mt-0.5 shrink-0" size={16} />
                      Proprietary technology with 2 pending patents.
                    </li>
                    <li className="flex items-start gap-3 text-sm text-slate-600">
                      <CheckCircle className="text-emerald-500 mt-0.5 shrink-0" size={16} />
                      Market size projected to reach $12B by 2028.
                    </li>
                    <li className="flex items-start gap-3 text-sm text-slate-600">
                      <CheckCircle className="text-emerald-500 mt-0.5 shrink-0" size={16} />
                      Lead investor committed 30% of the round.
                    </li>
                  </ul>
                </div>
              </div>

              <div className="space-y-6">
                <div className="bg-white p-6 rounded-xl shadow-sm border border-slate-200">
                  <h3 className="text-sm font-semibold text-slate-400 uppercase tracking-wider mb-4">Round Details</h3>
                  <div className="space-y-4">
                    <div>
                      <div className="flex justify-between text-sm mb-1">
                        <span className="text-slate-500">Raised</span>
                        <span className="font-bold text-slate-800">{formatCurrency(startup.raised)}</span>
                      </div>
                      <div className="w-full bg-slate-100 rounded-full h-2">
                        <div className="bg-emerald-500 h-2 rounded-full" style={{ width: `${getProgress(startup.raised, startup.goal)}%` }}></div>
                      </div>
                      <div className="flex justify-between text-xs mt-1 text-slate-400">
                        <span>{getProgress(startup.raised, startup.goal)}%</span>
                        <span>Goal: {formatCurrency(startup.goal)}</span>
                      </div>
                    </div>
                    <div className="pt-4 border-t border-slate-100 grid grid-cols-2 gap-4">
                      <div>
                        <div className="text-xs text-slate-400">Valuation (Cap)</div>
                        <div className="font-bold text-slate-800">{formatCurrency(startup.valuation)}</div>
                      </div>
                      <div>
                        <div className="text-xs text-slate-400">Min Check</div>
                        <div className="font-bold text-slate-800">{formatCurrency(startup.minCheck)}</div>
                      </div>
                      <div>
                        <div className="text-xs text-slate-400">Stage</div>
                        <div className="font-bold text-slate-800">{startup.stage}</div>
                      </div>
                      <div>
                        <div className="text-xs text-slate-400">Revenue</div>
                        <div className="font-bold text-slate-800">{startup.revenue}</div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          )}

          {activeTab === 'team' && (
            <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
              {startup.team.map((member, idx) => (
                <div key={idx} className="bg-white p-4 rounded-xl shadow-sm border border-slate-200 flex items-center gap-4">
                  <div className="w-12 h-12 rounded-full bg-slate-200 flex items-center justify-center text-slate-500 font-bold">
                    {member.name[0]}
                  </div>
                  <div>
                    <h4 className="font-bold text-slate-800">{member.name}</h4>
                    <p className="text-sm text-indigo-600 font-medium">{member.role}</p>
                    <p className="text-xs text-slate-500 mt-1">Ex-{member.ex}</p>
                  </div>
                </div>
              ))}
            </div>
          )}

          {activeTab === 'dataroom' && (
            <div className="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
              {[
                { name: 'Pitch Deck (Series A).pdf', type: 'PDF', date: 'Oct 12, 2023', size: '2.4 MB' },
                { name: 'Financial Projections FY24-26.xlsx', type: 'Excel', date: 'Oct 10, 2023', size: '1.1 MB' },
                { name: 'Cap Table.pdf', type: 'PDF', date: 'Sep 28, 2023', size: '850 KB' },
                { name: 'Incorporation Documents.zip', type: 'ZIP', date: 'Aug 15, 2023', size: '4.2 MB' }
              ].map((file, idx) => (
                <div key={idx} className="flex items-center justify-between p-4 border-b border-slate-100 last:border-0 hover:bg-slate-50 group">
                  <div className="flex items-center gap-3">
                    <div className="w-10 h-10 rounded-lg bg-blue-50 text-blue-600 flex items-center justify-center">
                      <FileText size={20} />
                    </div>
                    <div>
                      <p className="text-sm font-medium text-slate-800 group-hover:text-indigo-600 transition-colors">{file.name}</p>
                      <p className="text-xs text-slate-400">{file.type} • {file.date}</p>
                    </div>
                  </div>
                  <div className="flex items-center gap-4">
                    <span className="text-xs text-slate-400">{file.size}</span>
                    <button className="p-2 text-slate-400 hover:text-indigo-600 hover:bg-indigo-50 rounded-full transition-colors">
                      <Download size={16} />
                    </button>
                  </div>
                </div>
              ))}
              <div className="p-4 bg-slate-50 text-center">
                <p className="text-xs text-slate-500 flex items-center justify-center gap-2">
                  <Lock size={12} /> Access logged and monitored for compliance.
                </p>
              </div>
            </div>
          )}

           {activeTab === 'financials' && (
             <div className="flex flex-col items-center justify-center h-64 bg-white rounded-xl border border-slate-200">
               <Lock className="text-slate-300 mb-3" size={48} />
               <h3 className="font-bold text-slate-800">Confidential Financials</h3>
               <p className="text-slate-500 text-sm mt-2 max-w-md text-center">
                 Detailed P&L and Cash Flow statements are available only to investors in the "Due Diligence" stage.
               </p>
               <button className="mt-4 text-indigo-600 text-sm font-medium hover:underline">Request Access</button>
             </div>
           )}
        </div>
      </div>
    </div>
  );
};

// 4. Main App Container
export default function VentureFlowApp() {
  const [userType, setUserType] = useState('investor'); // 'investor' or 'startup'
  const [activeTab, setActiveTab] = useState('marketplace');
  const [selectedStartup, setSelectedStartup] = useState(null);
  const [pipeline, setPipeline] = useState(INITIAL_PIPELINE);
  const [notification, setNotification] = useState(null);

  const handleInvest = (startup) => {
    if (pipeline.find(p => p.startupId === startup.id)) {
      setNotification({ type: 'error', message: 'Already in your pipeline!' });
      return;
    }
    setPipeline([...pipeline, { startupId: startup.id, status: 'Screening', added: 'Just now' }]);
    setNotification({ type: 'success', message: `${startup.name} added to Deal Flow.` });
    setTimeout(() => setNotification(null), 3000);
    setSelectedStartup(null);
  };

  // Startup View Component (simplified)
  const StartupDashboard = () => (
    <div className="space-y-6">
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm">
          <div className="flex items-center justify-between mb-4">
            <h3 className="text-sm font-medium text-slate-500">Amount Raised</h3>
            <DollarSign className="text-emerald-500" size={20} />
          </div>
          <p className="text-3xl font-bold text-slate-800">$850,000</p>
          <div className="mt-2 w-full bg-slate-100 rounded-full h-1.5">
            <div className="bg-emerald-500 h-1.5 rounded-full" style={{ width: '42%' }}></div>
          </div>
          <p className="text-xs text-slate-400 mt-2">42% of $2.0M Goal</p>
        </div>

        <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm">
          <div className="flex items-center justify-between mb-4">
            <h3 className="text-sm font-medium text-slate-500">Pipeline Activity</h3>
            <TrendingUp className="text-blue-500" size={20} />
          </div>
          <p className="text-3xl font-bold text-slate-800">14</p>
          <p className="text-xs text-emerald-600 mt-2 flex items-center gap-1">
            <ArrowUpRight size={12} /> +3 this week
          </p>
          <p className="text-xs text-slate-400 mt-1">Active Investor Conversations</p>
        </div>

        <div className="bg-white p-6 rounded-xl border border-slate-200 shadow-sm">
          <div className="flex items-center justify-between mb-4">
            <h3 className="text-sm font-medium text-slate-500">Data Room</h3>
            <Users className="text-purple-500" size={20} />
          </div>
          <p className="text-3xl font-bold text-slate-800">8</p>
          <p className="text-xs text-slate-400 mt-2">Investors viewing documents</p>
        </div>
      </div>

      <div className="bg-white rounded-xl border border-slate-200 shadow-sm">
        <div className="p-6 border-b border-slate-100">
          <h3 className="font-bold text-slate-800">Interested Investors</h3>
        </div>
        <div className="p-0">
          {[
            { name: "Apex Ventures", type: "VC Fund", status: "Due Diligence", lastActive: "2h ago" },
            { name: "Michael Chen", type: "Angel", status: "First Meeting", lastActive: "1d ago" },
            { name: "BlueHorizon Capital", type: "Family Office", status: "Screening", lastActive: "3d ago" }
          ].map((inv, i) => (
            <div key={i} className="flex items-center justify-between p-4 border-b border-slate-50 last:border-0 hover:bg-slate-50">
              <div className="flex items-center gap-3">
                <div className="w-10 h-10 rounded-full bg-indigo-100 text-indigo-600 flex items-center justify-center font-bold text-sm">
                  {inv.name[0]}
                </div>
                <div>
                  <p className="font-medium text-slate-800">{inv.name}</p>
                  <p className="text-xs text-slate-500">{inv.type}</p>
                </div>
              </div>
              <div className="flex items-center gap-4">
                <span className={`px-3 py-1 rounded-full text-xs font-medium
                  ${inv.status === 'Due Diligence' ? 'bg-purple-100 text-purple-700' :
                    inv.status === 'First Meeting' ? 'bg-blue-100 text-blue-700' : 
                    'bg-slate-100 text-slate-600'}`}>
                  {inv.status}
                </span>
                <span className="text-xs text-slate-400 flex items-center gap-1">
                  <Clock size={12} /> {inv.lastActive}
                </span>
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  );

  return (
    <div className="min-h-screen bg-slate-50 font-sans text-slate-900 flex">
      
      {/* Notification Toast */}
      {notification && (
        <div className="fixed top-4 right-4 z-50 animate-in slide-in-from-top-2">
          <div className={`px-4 py-3 rounded-lg shadow-lg flex items-center gap-3 text-sm font-medium text-white
            ${notification.type === 'success' ? 'bg-emerald-600' : 'bg-red-500'}`}>
            {notification.type === 'success' ? <CheckCircle size={16} /> : <X size={16} />}
            {notification.message}
          </div>
        </div>
      )}

      {/* Sidebar Navigation */}
      <Sidebar activeTab={activeTab} setActiveTab={setActiveTab} userType={userType} />

      {/* Main Content */}
      <main className="flex-1 md:ml-64 transition-all duration-300">
        
        {/* Top Bar */}
        <header className="h-16 bg-white border-b border-slate-200 sticky top-0 z-10 flex items-center justify-between px-8">
          <div className="flex items-center gap-4">
            <h1 className="text-xl font-bold text-slate-800 capitalize">
              {activeTab.replace(/([A-Z])/g, ' $1').trim()}
            </h1>
          </div>
          
          <div className="flex items-center gap-6">
            <div className="relative hidden sm:block">
              <Search className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" size={16} />
              <input 
                type="text" 
                placeholder="Search deals, companies..." 
                className="pl-10 pr-4 py-2 bg-slate-100 rounded-lg text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 w-64 transition-all"
              />
            </div>
            
            <button 
              onClick={() => setUserType(prev => prev === 'investor' ? 'startup' : 'investor')}
              className="text-xs font-semibold text-indigo-600 bg-indigo-50 border border-indigo-100 px-3 py-1.5 rounded-md hover:bg-indigo-100 transition-colors"
            >
              Switch View: {userType === 'investor' ? 'Startup' : 'Investor'}
            </button>

            <button className="relative text-slate-400 hover:text-slate-600">
              <Bell size={20} />
              <span className="absolute top-0 right-0 w-2 h-2 bg-red-500 rounded-full"></span>
            </button>
            
            <div className="w-8 h-8 rounded-full bg-indigo-600 flex items-center justify-center text-white font-bold shadow-md">
              {userType === 'investor' ? 'JD' : 'ER'}
            </div>
          </div>
        </header>

        {/* Content Body */}
        <div className="p-8">
          
          {/* INVESTOR: Marketplace View */}
          {userType === 'investor' && activeTab === 'marketplace' && (
            <div>
              <div className="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-8 gap-4">
                <div>
                  <h2 className="text-2xl font-bold text-slate-800">Live Deals</h2>
                  <p className="text-slate-500">Curated investment opportunities matching your thesis.</p>
                </div>
                <button className="flex items-center gap-2 px-4 py-2 bg-white border border-slate-200 rounded-lg text-sm font-medium text-slate-600 hover:bg-slate-50">
                  <Filter size={16} /> Filters
                </button>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                {MOCK_STARTUPS.map((startup) => (
                  <div key={startup.id} className="bg-white rounded-xl border border-slate-200 shadow-sm hover:shadow-xl transition-all duration-300 overflow-hidden group flex flex-col h-full">
                    <div className="h-32 bg-slate-100 relative overflow-hidden">
                      <div className="absolute inset-0 bg-gradient-to-tr from-indigo-500/10 to-blue-500/10 group-hover:scale-105 transition-transform duration-500"></div>
                      <span className="absolute top-4 left-4 bg-white/90 backdrop-blur-sm px-2 py-1 rounded text-xs font-bold text-indigo-600 shadow-sm">
                        {startup.industry}
                      </span>
                      <span className="absolute top-4 right-4 bg-slate-900/80 text-white px-2 py-1 rounded text-xs font-medium">
                        {startup.stage}
                      </span>
                    </div>
                    
                    <div className="p-5 flex-1 flex flex-col">
                      <div className="flex items-center justify-between mb-2">
                        <h3 className="font-bold text-xl text-slate-900">{startup.name}</h3>
                        <div className="text-xs text-slate-500 flex items-center gap-1">
                          <Building2 size={12} /> {startup.location}
                        </div>
                      </div>
                      
                      <p className="text-slate-600 text-sm mb-4 line-clamp-2 flex-1">{startup.description}</p>
                      
                      <div className="space-y-3">
                        <div className="w-full bg-slate-100 rounded-full h-2">
                          <div className="bg-indigo-600 h-2 rounded-full transition-all duration-1000" style={{ width: `${getProgress(startup.raised, startup.goal)}%` }}></div>
                        </div>
                        <div className="flex justify-between text-xs font-medium">
                          <span className="text-slate-700">{formatCurrency(startup.raised)} raised</span>
                          <span className="text-slate-400">Target: {formatCurrency(startup.goal)}</span>
                        </div>
                      </div>
                    </div>

                    <div className="p-4 border-t border-slate-100 bg-slate-50/50">
                      <button 
                        onClick={() => setSelectedStartup(startup)}
                        className="w-full py-2.5 bg-white border border-slate-300 hover:border-indigo-500 hover:text-indigo-600 text-slate-700 font-medium rounded-lg transition-all text-sm shadow-sm"
                      >
                        View Details
                      </button>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}

          {/* INVESTOR: Deal Flow / Pipeline View */}
          {userType === 'investor' && activeTab === 'dealflow' && (
            <div className="h-[calc(100vh-12rem)]">
              <div className="mb-6">
                <h2 className="text-2xl font-bold text-slate-800">Deal Flow Pipeline</h2>
                <p className="text-slate-500">Drag and drop functionality simulated via buttons.</p>
              </div>
              <DealFlowPipeline 
                pipeline={pipeline} 
                setPipeline={setPipeline} 
                onSelectStartup={setSelectedStartup}
              />
            </div>
          )}

          {/* INVESTOR: Dashboard/Portfolio View (Placeholder) */}
          {userType === 'investor' && (activeTab === 'dashboard' || activeTab === 'portfolio') && (
             <div className="flex flex-col items-center justify-center h-96 text-center">
               <div className="w-16 h-16 bg-slate-100 rounded-full flex items-center justify-center mb-4 text-slate-400">
                 <PieChart size={32} />
               </div>
               <h3 className="text-xl font-bold text-slate-800">Portfolio Dashboard</h3>
               <p className="text-slate-500 max-w-md mt-2">
                 Visualize internal rate of return (IRR), capital deployment, and portfolio distribution across industries.
               </p>
               <button onClick={() => setActiveTab('marketplace')} className="mt-6 text-indigo-600 font-medium hover:underline">
                 Explore Marketplace
               </button>
             </div>
          )}

          {/* STARTUP: Dashboard */}
          {userType === 'startup' && activeTab === 'dashboard' && <StartupDashboard />}
          
          {/* STARTUP: Other tabs placeholders */}
          {userType === 'startup' && activeTab !== 'dashboard' && (
            <div className="flex flex-col items-center justify-center h-96 text-center">
              <div className="w-16 h-16 bg-slate-100 rounded-full flex items-center justify-center mb-4 text-slate-400">
                <Settings size={32} />
              </div>
              <h3 className="text-xl font-bold text-slate-800">Company Management</h3>
              <p className="text-slate-500 max-w-md mt-2">
                Manage your data room permissions, cap table, and corporate governance documents here.
              </p>
            </div>
          )}

        </div>
      </main>

      {/* Modals */}
      {selectedStartup && (
        <StartupDetail 
          startup={selectedStartup} 
          onClose={() => setSelectedStartup(null)} 
          onInvest={handleInvest}
          isInPipeline={pipeline.some(p => p.startupId === selectedStartup.id)}
        />
      )}

    </div>
  );
}

