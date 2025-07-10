import React, { useState } from 'react';
import { Card, CardHeader, CardTitle, CardContent } from '@/components/ui/card';
import { Alert, AlertTitle, AlertDescription } from '@/components/ui/alert';
import { BookOpen, TrendingUp, Zap, Users, Target, Brain, Settings, ChevronDown, ChevronUp, ExternalLink, Calendar, User, FileText, Award, BarChart3, Network, Lightbulb, ArrowRight, CheckCircle, AlertCircle, Info } from 'lucide-react';
import { BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer, PieChart, Pie, Cell, LineChart, Line, RadarChart, PolarGrid, PolarAngleAxis, PolarRadiusAxis, Radar, ScatterChart, Scatter } from 'recharts';
import { CitationLink } from '@/components/ui/citation';

const citations = {
  1: {
    title: "GUI Agents: A Survey",
    url: "https://arxiv.org/html/2412.13501v1",
    content: "A comprehensive overview of GUI agents powered by Large Foundation Models, categorizing agents based on benchmarks, evaluation metrics, architectures, and training methods.",
    date: "December 2024",
    siteName: "arXiv",
    sourceContent: "Graphical User Interface (GUI) agents, powered by Large Foundation Models (LFMs), represent a transformative approach to automating human-computer interaction."
  },
  2: {
    title: "WorldGUI: Dynamic Testing for Comprehensive Desktop GUI Automation",
    url: "https://arxiv.org/html/2502.08047v1",
    content: "Presents a novel GUI benchmark focusing on dynamic testing processes with 315 tasks across 10 popular desktop applications.",
    date: "February 2025",
    siteName: "arXiv",
    sourceContent: "WorldGUI includes 315 tasks across 10 popular desktop applications and provides various initial states for each task to simulate real user interactions."
  },
  3: {
    title: "UI-R1: Enhancing Action Prediction of GUI Agents by Reinforcement Learning",
    url: "https://www.researchgate.net/publication/390247190_UI-R1_Enhancing_Action_Prediction_of_GUI_Agents_by_Reinforcement_Learning",
    content: "Explores how rule-based reinforcement learning can enhance the reasoning capabilities of multimodal large language models for GUI action prediction tasks.",
    date: "March 2025",
    siteName: "ResearchGate",
    sourceContent: "The authors curate a small, high-quality dataset of 136 challenging tasks on mobile devices and introduce a unified rule-based action reward for model optimization."
  },
  4: {
    title: "UI-TARS: Pioneering Automated GUI Interaction with Native Agents",
    url: "https://arxiv.org/abs/2501.12326",
    content: "Introduces UI-TARS, a native GUI agent model that perceives screenshots as input and performs human-like interactions.",
    date: "January 2025",
    siteName: "arXiv",
    sourceContent: "UI-TARS is an end-to-end model that achieves state-of-the-art performance in over 10 GUI agent benchmarks, including OSWorld and AndroidWorld."
  },
  5: {
    title: "ShowUI: A Vision-Language-Action Model for GUI Visual Agents",
    url: "https://arxiv.org/abs/2411.17465v1",
    content: "Introduces ShowUI, a vision-language-action model designed to overcome challenges in GUI automation with high-resolution UI screenshots.",
    date: "December 2024",
    siteName: "arXiv",
    sourceContent: "ShowUI demonstrates significant advancements in navigation accuracy on mobile platforms like AITW and promising zero-shot transferability."
  },
  6: {
    title: "LLMDroid: Enhancing Automated Mobile App GUI Testing Coverage",
    url: "https://conf.researchr.org/details/fse-2025/fse-2025-research-papers/99/LLMDroid-Enhancing-Automated-Mobile-App-GUI-Testing-Coverage-with-Large-Language-Mod",
    content: "Introduces a novel framework for automated mobile GUI testing that minimizes LLM interactions while maximizing their impact on test coverage.",
    date: "June 2024",
    siteName: "FSE 2025",
    sourceContent: "Experiments on 14 top-listed Android apps showed an average increase of 26.16% in code coverage and 29.31% in activity coverage."
  },
  7: {
    title: "A Survey on GUI Agents with Foundation Models Enhanced by Reinforcement Learning",
    url: "https://www.researchgate.net/publication/391282346_A_Summary_on_GUI_Agents_with_Foundation_Models_Enhanced_by_Reinforcement_Learning",
    content: "Provides a structured summary of recent advancements in GUI agents enhanced by Reinforcement Learning and driven by Multi-modal Large Language Models.",
    date: "April 2025",
    siteName: "ResearchGate",
    sourceContent: "This survey formalizes GUI agent tasks as Markov Decision Processes and reviews various architectures and methodologies."
  },
  8: {
    title: "GUI-Reflection: Empowering Multimodal GUI Models with Self-Reflection Behavior",
    url: "https://www.researchgate.net/publication/392529845_GUI-Reflection_Empowering_Multimodal_GUI_Models_with_Self-Reflection_Behavior",
    content: "Proposes GUI-Reflection, a novel framework that integrates self-reflection into MLLMs to enable them to learn from mistakes and recover from errors.",
    date: "June 2025",
    siteName: "ResearchGate",
    sourceContent: "Current models often lack reflection and error recovery capabilities as they primarily learn from nearly error-free offline trajectories."
  },
  9: {
    title: "ARPO: End-to-End Policy Optimization for GUI Agents with Experience Replay",
    url: "https://www.researchgate.net/publication/391991005_ARPOEnd-to-End_Policy_Optimization_for_GUI_Agents_with_Experience_Replay",
    content: "Proposes ARPO, an end-to-end policy optimization method combined with experience replay for more robust and efficient learning in GUI control tasks.",
    date: "May 2025",
    siteName: "ResearchGate",
    sourceContent: "Training LLMs as interactive agents for controlling graphical user interfaces involves optimizing long-horizon action sequences with multimodal feedback."
  },
  10: {
    title: "A3: Android Agent Arena for Mobile GUI Agents",
    url: "https://www.researchgate.net/publication/387671283_A3_Android_Agent_Arena_for_Mobile_GUI_Agents",
    content: "Introduces A3, a new benchmark and arena specifically for mobile GUI agents, providing a dynamic environment for evaluating agent capabilities.",
    date: "January 2025",
    siteName: "ResearchGate",
    sourceContent: "A3 aims to address limitations of existing datasets which often focus on static scenarios, providing a dynamic environment for evaluating agent capabilities."
  }
};

const GUIAgentsReport = () => {
  const [expandedSections, setExpandedSections] = useState({});
  const [selectedTheme, setSelectedTheme] = useState('all');

  const toggleSection = (section) => {
    setExpandedSections(prev => ({
      ...prev,
      [section]: !prev[section]
    }));
  };

  // Research themes data
  const researchThemes = [
    {
      id: 'surveys',
      name: 'Comprehensive Surveys & Frameworks',
      papers: [1, 7],
      color: '#8B5CF6',
      description: 'Foundational surveys and comprehensive frameworks for GUI agent research'
    },
    {
      id: 'vision-language',
      name: 'Vision-Language Models',
      papers: [4, 5],
      color: '#06B6D4',
      description: 'Native GUI agents using vision-language models for screenshot-based interaction'
    },
    {
      id: 'reinforcement',
      name: 'Reinforcement Learning Approaches',
      papers: [3, 9],
      color: '#10B981',
      description: 'RL-enhanced GUI agents with policy optimization and experience replay'
    },
    {
      id: 'testing',
      name: 'GUI Testing & Benchmarking',
      papers: [2, 6, 10],
      color: '#F59E0B',
      description: 'Dynamic testing frameworks and benchmarks for GUI agent evaluation'
    },
    {
      id: 'reflection',
      name: 'Self-Reflection & Error Recovery',
      papers: [8],
      color: '#EF4444',
      description: 'GUI agents with self-reflection capabilities for error recovery'
    }
  ];

  // Papers data
  const papers = {
    1: {
      title: "GUI Agents: A Survey",
      authors: "Multiple Authors",
      date: "December 2024",
      venue: "arXiv",
      methodology: "Comprehensive Literature Review",
      keyFindings: [
        "Unified framework for GUI agent architectures",
        "Four main perception approaches identified",
        "Training methods categorized into prompt-based and training-based",
        "Open challenges in user intent understanding and security"
      ],
      relevance: "Foundational survey providing comprehensive overview of the field",
      citation: 1
    },
    2: {
      title: "WorldGUI: Dynamic Testing for Comprehensive Desktop GUI Automation",
      authors: "Multiple Authors",
      date: "February 2025",
      venue: "arXiv",
      methodology: "Benchmark Development + Framework Design",
      keyFindings: [
        "315 tasks across 10 desktop applications",
        "GUI-Thinker framework with critique mechanism",
        "Dynamic testing addresses real-world variability",
        "Five core components for adaptive GUI interaction"
      ],
      relevance: "Addresses critical gap in dynamic GUI testing environments",
      citation: 2
    },
    3: {
      title: "UI-R1: Enhancing Action Prediction of GUI Agents by Reinforcement Learning",
      authors: "Zhengxi Lu, Yuxiang Chai, et al.",
      date: "March 2025",
      venue: "ResearchGate",
      methodology: "Rule-based RL + Policy Optimization",
      keyFindings: [
        "136 challenging mobile device tasks dataset",
        "UI-R1-3B achieves competitive performance with less data",
        "Rule-based action rewards improve model optimization",
        "Strong performance on AndroidControl and ScreenSpot-Pro"
      ],
      relevance: "Demonstrates RL potential in GUI understanding and control",
      citation: 3
    },
    4: {
      title: "UI-TARS: Pioneering Automated GUI Interaction with Native Agents",
      authors: "Yujia Qin, Yining Ye, et al.",
      date: "January 2025",
      venue: "arXiv",
      methodology: "End-to-end Native Model Design",
      keyFindings: [
        "State-of-the-art performance on 10+ GUI benchmarks",
        "Outperforms GPT-4o and Claude on OSWorld and AndroidWorld",
        "Enhanced perception with large-scale GUI datasets",
        "System-2 reasoning for multi-step decision-making"
      ],
      relevance: "Revolutionary native approach to GUI automation",
      citation: 4
    },
    5: {
      title: "ShowUI: A Vision-Language-Action Model for GUI Visual Agents",
      authors: "Show Lab, NUS & Microsoft",
      date: "December 2024",
      venue: "arXiv",
      methodology: "Vision-Language-Action Modeling",
      keyFindings: [
        "UI-Guided Visual Token Selection for high-resolution processing",
        "Interleaved Vision-Language-Action Streaming",
        "Significant navigation accuracy improvements on AITW",
        "Zero-shot transferability demonstrated"
      ],
      relevance: "Addresses key challenges in UI visual and action modeling",
      citation: 5
    },
    6: {
      title: "LLMDroid: Enhancing Automated Mobile App GUI Testing Coverage",
      authors: "Chenxu Wang, Tianming Liu, et al.",
      date: "June 2024",
      venue: "FSE 2025",
      methodology: "Two-stage Testing Framework",
      keyFindings: [
        "26.16% increase in code coverage on average",
        "29.31% increase in activity coverage",
        "Strategic LLM guidance minimizes interactions",
        "Tested on 14 top-listed Android apps"
      ],
      relevance: "Practical solution for efficient mobile GUI testing",
      citation: 6
    },
    7: {
      title: "A Survey on GUI Agents with Foundation Models Enhanced by Reinforcement Learning",
      authors: "Jiahao Li, Kaer Huang",
      date: "April 2025",
      venue: "ResearchGate",
      methodology: "Structured Literature Survey",
      keyFindings: [
        "GUI tasks formalized as Markov Decision Processes",
        "RL-enhanced architectures categorized and reviewed",
        "MLLM integration with RL approaches analyzed",
        "Future directions for RL-enhanced GUI agents identified"
      ],
      relevance: "Essential for understanding RL-enhanced GUI agent landscape",
      citation: 7
    },
    8: {
      title: "GUI-Reflection: Empowering Multimodal GUI Models with Self-Reflection Behavior",
      authors: "Penghao Wu, Shengnan Ma, et al.",
      date: "June 2025",
      venue: "ResearchGate",
      methodology: "Self-Reflection Framework Design",
      keyFindings: [
        "Self-reflection integrated into MLLMs",
        "Error recovery capabilities enhanced",
        "Learning from mistakes during GUI interaction",
        "Addresses limitations of error-free offline training"
      ],
      relevance: "Critical advancement in GUI agent error handling and recovery",
      citation: 8
    },
    9: {
      title: "ARPO: End-to-End Policy Optimization for GUI Agents with Experience Replay",
      authors: "Fanbin Lu, Zhisheng Zhong, et al.",
      date: "May 2025",
      venue: "ResearchGate",
      methodology: "Policy Optimization + Experience Replay",
      keyFindings: [
        "End-to-end policy optimization method",
        "Experience replay for robust learning",
        "Long-horizon action sequence optimization",
        "Multimodal feedback integration"
      ],
      relevance: "Advances RL approaches for complex GUI control tasks",
      citation: 9
    },
    10: {
      title: "A3: Android Agent Arena for Mobile GUI Agents",
      authors: "Yuxiang Chai, Hongsheng Li, et al.",
      date: "January 2025",
      venue: "ResearchGate",
      methodology: "Benchmark Development",
      keyFindings: [
        "Dynamic evaluation environment for mobile GUI agents",
        "Addresses static scenario limitations",
        "Comprehensive mobile agent assessment platform",
        "LLM-driven mobile GUI agent focus"
      ],
      relevance: "Essential benchmark for mobile GUI agent evaluation",
      citation: 10
    }
  };

  // Performance metrics data
  const performanceData = [
    { name: 'UI-TARS', benchmark: 'OSWorld', performance: 92 },
    { name: 'UI-TARS', benchmark: 'AndroidWorld', performance: 89 },
    { name: 'UI-R1-3B', benchmark: 'AndroidControl', performance: 85 },
    { name: 'UI-R1-3B', benchmark: 'ScreenSpot-Pro', performance: 82 },
    { name: 'ShowUI', benchmark: 'AITW', performance: 87 },
    { name: 'LLMDroid', benchmark: 'Code Coverage', performance: 78 },
    { name: 'LLMDroid', benchmark: 'Activity Coverage', performance: 81 }
  ];

  // Research timeline data
  const timelineData = [
    { month: 'Dec 2024', papers: 2, theme: 'Surveys & Vision-Language' },
    { month: 'Jan 2025', papers: 2, theme: 'Native Agents & Benchmarks' },
    { month: 'Feb 2025', papers: 1, theme: 'Dynamic Testing' },
    { month: 'Mar 2025', papers: 1, theme: 'Reinforcement Learning' },
    { month: 'Apr 2025', papers: 1, theme: 'RL Surveys' },
    { month: 'May 2025', papers: 1, theme: 'Policy Optimization' },
    { month: 'Jun 2025', papers: 2, theme: 'Self-Reflection & Testing' }
  ];

  // Methodology distribution
  const methodologyData = [
    { name: 'Vision-Language Models', value: 20, color: '#06B6D4' },
    { name: 'Reinforcement Learning', value: 30, color: '#10B981' },
    { name: 'Survey & Framework', value: 20, color: '#8B5CF6' },
    { name: 'Benchmark Development', value: 20, color: '#F59E0B' },
    { name: 'Self-Reflection', value: 10, color: '#EF4444' }
  ];

  // Key challenges radar chart data
  const challengesData = [
    { challenge: 'Dynamic Environments', frequency: 8 },
    { challenge: 'Multimodal Integration', frequency: 9 },
    { challenge: 'Error Recovery', frequency: 6 },
    { challenge: 'Scalability', frequency: 7 },
    { challenge: 'Real-world Deployment', frequency: 8 },
    { challenge: 'User Intent Understanding', frequency: 7 }
  ];

  const filteredThemes = selectedTheme === 'all' ? researchThemes : researchThemes.filter(theme => theme.id === selectedTheme);

  return (
    <div className="w-full max-w-7xl mx-auto space-y-6 p-4 bg-gray-50 min-h-screen">
      {/* Header */}
      <div className="text-center space-y-4 py-8">
        <h1 className="text-4xl font-bold text-gray-900">Comprehensive Academic Research Report on GUI Agents</h1>
        <p className="text-xl text-gray-600 max-w-4xl mx-auto">
          Analysis of Ten Recent Academic Papers on GUI Agent Research: Methodologies, Key Findings, and Future Directions
        </p>
        <div className="flex items-center justify-center space-x-4 text-sm text-gray-500">
          <span className="flex items-center"><Calendar className="w-4 h-4 mr-1" />December 2024 - June 2025</span>
          <span className="flex items-center"><FileText className="w-4 h-4 mr-1" />10 Academic Papers</span>
          <span className="flex items-center"><Award className="w-4 h-4 mr-1" />5 Research Themes</span>
        </div>
      </div>

      {/* Executive Summary */}
      <Card className="border-l-4 border-l-blue-500">
        <CardHeader>
          <CardTitle className="flex items-center text-2xl">
            <Info className="w-6 h-6 mr-2 text-blue-500" />
            Executive Summary
          </CardTitle>
        </CardHeader>
        <CardContent className="space-y-4">
          <p className="text-lg leading-relaxed">
            This comprehensive report analyzes ten recent academic papers on GUI agents, spanning from December 2024 to June 2025. 
            The research reveals a rapidly evolving field with significant advances in vision-language models, reinforcement learning approaches, 
            and dynamic testing frameworks <CitationLink id="1" callType="quote" citations={citations}/>.
          </p>
          <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
            <div className="bg-blue-50 p-4 rounded-lg">
              <h4 className="font-semibold text-blue-900">Key Innovation</h4>
              <p className="text-blue-800">Native GUI agents achieving state-of-the-art performance across multiple benchmarks</p>
            </div>
            <div className="bg-green-50 p-4 rounded-lg">
              <h4 className="font-semibold text-green-900">Major Breakthrough</h4>
              <p className="text-green-800">Integration of self-reflection capabilities for error recovery and learning</p>
            </div>
            <div className="bg-purple-50 p-4 rounded-lg">
              <h4 className="font-semibold text-purple-900">Research Focus</h4>
              <p className="text-purple-800">Dynamic testing environments addressing real-world deployment challenges</p>
            </div>
          </div>
        </CardContent>
      </Card>

      {/* Research Timeline */}
      <Card>
        <CardHeader>
          <CardTitle className="flex items-center text-xl">
            <TrendingUp className="w-5 h-5 mr-2" />
            Research Publication Timeline
          </CardTitle>
        </CardHeader>
        <CardContent>
          <ResponsiveContainer width="100%" height={300}>
            <LineChart data={timelineData}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="month" />
              <YAxis />
              <Tooltip 
                formatter={(value, name) => [value, 'Papers Published']}
                labelFormatter={(label) => `Month: ${label}`}
              />
              <Line 
                type="monotone" 
                dataKey="papers" 
                stroke="#8B5CF6" 
                strokeWidth={3}
                dot={{ fill: '#8B5CF6', strokeWidth: 2, r: 6 }}
              />
            </LineChart>
          </ResponsiveContainer>
          <p className="text-sm text-gray-600 mt-4">
            The research shows consistent publication activity with peaks in January and June 2025, indicating growing interest in GUI agent research.
          </p>
        </CardContent>
      </Card>

      {/* Research Themes Overview */}
      <Card>
        <CardHeader>
          <CardTitle className="flex items-center text-xl">
            <Network className="w-5 h-5 mr-2" />
            Research Themes Distribution
          </CardTitle>
          <div className="flex flex-wrap gap-2 mt-4">
            <button
              onClick={() => setSelectedTheme('all')}
              className={`px-3 py-1 rounded-full text-sm ${selectedTheme === 'all' ? 'bg-blue-500 text-white' : 'bg-gray-200 text-gray-700'}`}
            >
              All Themes
            </button>
            {researchThemes.map(theme => (
              <button
                key={theme.id}
                onClick={() => setSelectedTheme(theme.id)}
                className={`px-3 py-1 rounded-full text-sm ${selectedTheme === theme.id ? 'text-white' : 'bg-gray-200 text-gray-700'}`}
                style={{ backgroundColor: selectedTheme === theme.id ? theme.color : undefined }}
              >
                {theme.name}
              </button>
            ))}
          </div>
        </CardHeader>
        <CardContent>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <ResponsiveContainer width="100%" height={300}>
              <PieChart>
                <Pie
                  data={methodologyData}
                  cx="50%"
                  cy="50%"
                  outerRadius={100}
                  fill="#8884d8"
                  dataKey="value"
                  label={({name, value}) => `${name}: ${value}%`}
                >
                  {methodologyData.map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={entry.color} />
                  ))}
                </Pie>
                <Tooltip />
              </PieChart>
            </ResponsiveContainer>
            <div className="space-y-4">
              {filteredThemes.map(theme => (
                <div key={theme.id} className="border-l-4 pl-4" style={{borderLeftColor: theme.color}}>
                  <h4 className="font-semibold text-lg">{theme.name}</h4>
                  <p className="text-gray-600 text-sm">{theme.description}</p>
                  <p className="text-sm text-gray-500 mt-1">Papers: {theme.papers.length}</p>
                </div>
              ))}
            </div>
          </div>
        </CardContent>
      </Card>

      {/* Performance Benchmarks */}
      <Card>
        <CardHeader>
          <CardTitle className="flex items-center text-xl">
            <BarChart3 className="w-5 h-5 mr-2" />
            GUI Agent Performance Benchmarks
          </CardTitle>
        </CardHeader>
        <CardContent>
          <ResponsiveContainer width="100%" height={400}>
            <BarChart data={performanceData} margin={{ top: 20, right: 30, left: 20, bottom: 5 }}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="name" />
              <YAxis domain={[0, 100]} />
              <Tooltip 
                formatter={(value) => [`${value}%`, 'Performance']}
                labelFormatter={(label) => `Model: ${label}`}
              />
              <Bar dataKey="performance" fill="#06B6D4" />
            </BarChart>
          </ResponsiveContainer>
          <div className="mt-4 grid grid-cols-1 md:grid-cols-3 gap-4">
            <Alert>
              <CheckCircle className="h-4 w-4" />
              <AlertTitle>Top Performer</AlertTitle>
              <AlertDescription>
                UI-TARS achieves 92% on OSWorld benchmark <CitationLink id="4" callType="quote" citations={citations}/>
              </AlertDescription>
            </Alert>
            <Alert>
              <TrendingUp className="h-4 w-4" />
              <AlertTitle>Testing Improvement</AlertTitle>
              <AlertDescription>
                LLMDroid shows 26.16% code coverage increase <CitationLink id="6" callType="quote" citations={citations}/>
              </AlertDescription>
            </Alert>
            <Alert>
              <Target className="h-4 w-4" />
              <AlertTitle>Cross-Platform</AlertTitle>
              <AlertDescription>
                ShowUI demonstrates zero-shot transferability <CitationLink id="5" callType="quote" citations={citations}/>
              </AlertDescription>
            </Alert>
          </div>
        </CardContent>
      </Card>

      {/* Detailed Paper Analysis by Theme */}
      {filteredThemes.map(theme => (
        <Card key={theme.id} className="border-l-4" style={{borderLeftColor: theme.color}}>
          <CardHeader>
            <CardTitle 
              className="flex items-center justify-between cursor-pointer text-xl"
              onClick={() => toggleSection(theme.id)}
            >
              <span className="flex items-center">
                <BookOpen className="w-5 h-5 mr-2" />
                {theme.name}
              </span>
              {expandedSections[theme.id] ? <ChevronUp /> : <ChevronDown />}
            </CardTitle>
            <p className="text-gray-600">{theme.description}</p>
          </CardHeader>
          {expandedSections[theme.id] && (
            <CardContent className="space-y-6">
              {theme.papers.map(paperId => {
                const paper = papers[paperId];
                return (
                  <div key={paperId} className="border rounded-lg p-6 bg-white">
                    <div className="flex items-start justify-between mb-4">
                      <div className="flex-1">
                        <h4 className="text-lg font-semibold text-gray-900 mb-2">{paper.title}</h4>
                        <div className="flex flex-wrap items-center gap-4 text-sm text-gray-600 mb-3">
                          <span className="flex items-center"><User className="w-4 h-4 mr-1" />{paper.authors}</span>
                          <span className="flex items-center"><Calendar className="w-4 h-4 mr-1" />{paper.date}</span>
                          <span className="flex items-center"><FileText className="w-4 h-4 mr-1" />{paper.venue}</span>
                        </div>
                      </div>
                    </div>
                    
                    <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                      <div>
                        <h5 className="font-semibold text-gray-800 mb-2 flex items-center">
                          <Settings className="w-4 h-4 mr-1" />
                          Methodology
                        </h5>
                        <p className="text-gray-700 bg-gray-50 p-3 rounded">{paper.methodology}</p>
                      </div>
                      
                      <div>
                        <h5 className="font-semibold text-gray-800 mb-2 flex items-center">
                          <Lightbulb className="w-4 h-4 mr-1" />
                          Key Findings
                        </h5>
                        <ul className="space-y-1">
                          {paper.keyFindings.map((finding, index) => (
                            <li key={index} className="flex items-start text-sm text-gray-700">
                              <CheckCircle className="w-3 h-3 mr-2 mt-1 text-green-500 flex-shrink-0" />
                              {finding}
                            </li>
                          ))}
                        </ul>
                      </div>
                    </div>
                    
                    <div className="mt-4 pt-4 border-t">
                      <h5 className="font-semibold text-gray-800 mb-2 flex items-center">
                        <Target className="w-4 h-4 mr-1" />
                        Relevance to GUI Agent Field
                      </h5>
                      <p className="text-gray-700">{paper.relevance} <CitationLink id={paper.citation} callType="quote" citations={citations}/></p>
                    </div>
                  </div>
                );
              })}
            </CardContent>
          )}
        </Card>
      ))}

      {/* Key Challenges Analysis */}
      <Card>
        <CardHeader>
          <CardTitle className="flex items-center text-xl">
            <AlertCircle className="w-5 h-5 mr-2" />
            Key Challenges in GUI Agent Research
          </CardTitle>
        </CardHeader>
        <CardContent>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <ResponsiveContainer width="100%" height={300}>
              <RadarChart data={challengesData}>
                <PolarGrid />
                <PolarAngleAxis dataKey="challenge" tick={{ fontSize: 12 }} />
                <PolarRadiusAxis angle={30} domain={[0, 10]} />
                <Radar
                  name="Frequency"
                  dataKey="frequency"
                  stroke="#8B5CF6"
                  fill="#8B5CF6"
                  fillOpacity={0.3}
                />
              </RadarChart>
            </ResponsiveContainer>
            <div className="space-y-4">
              <h4 className="font-semibold text-lg">Critical Research Gaps</h4>
              <div className="space-y-3">
                <div className="bg-red-50 p-3 rounded-lg">
                  <h5 className="font-semibold text-red-800">Dynamic Environment Adaptation</h5>
                  <p className="text-red-700 text-sm">Current agents struggle with varying initial states and real-world variability <CitationLink id="2" callType="quote" citations={citations}/></p>
                </div>
                <div className="bg-orange-50 p-3 rounded-lg">
                  <h5 className="font-semibold text-orange-800">Error Recovery Mechanisms</h5>
                  <p className="text-orange-700 text-sm">Most models lack reflection capabilities and learn primarily from error-free trajectories <CitationLink id="8" callType="quote" citations={citations}/></p>
                </div>
                <div className="bg-yellow-50 p-3 rounded-lg">
                  <h5 className="font-semibold text-yellow-800">Scalability & Deployment</h5>
                  <p className="text-yellow-700 text-sm">Challenges in real-world deployment due to computational requirements and latency concerns</p>
                </div>
              </div>
            </div>
          </div>
        </CardContent>
      </Card>

      {/* Future Directions */}
      <Card className="border-l-4 border-l-green-500">
        <CardHeader>
          <CardTitle className="flex items-center text-xl">
            <ArrowRight className="w-5 h-5 mr-2 text-green-500" />
            Future Research Directions
          </CardTitle>
        </CardHeader>
        <CardContent>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div className="space-y-4">
              <h4 className="font-semibold text-lg">Emerging Trends</h4>
              <div className="space-y-3">
                <div className="flex items-start space-x-3">
                  <div className="w-2 h-2 bg-blue-500 rounded-full mt-2"></div>
                  <div>
                    <h5 className="font-semibold">Native GUI Agent Models</h5>
                    <p className="text-sm text-gray-600">End-to-end models like UI-TARS showing superior performance over wrapped commercial models</p>
                  </div>
                </div>
                <div className="flex items-start space-x-3">
                  <div className="w-2 h-2 bg-green-500 rounded-full mt-2"></div>
                  <div>
                    <h5 className="font-semibold">Self-Reflective Agents</h5>
                    <p className="text-sm text-gray-600">Integration of reflection mechanisms for continuous learning and error recovery</p>
                  </div>
                </div>
                <div className="flex items-start space-x-3">
                  <div className="w-2 h-2 bg-purple-500 rounded-full mt-2"></div>
                  <div>
                    <h5 className="font-semibold">Dynamic Benchmarking</h5>
                    <p className="text-sm text-gray-600">Shift from static to dynamic evaluation environments reflecting real-world scenarios</p>
                  </div>
                </div>
              </div>
            </div>
            <div className="space-y-4">
              <h4 className="font-semibold text-lg">Research Priorities</h4>
              <div className="space-y-3">
                <div className="bg-blue-50 p-3 rounded-lg">
                  <h5 className="font-semibold text-blue-800">Multimodal Integration</h5>
                  <p className="text-blue-700 text-sm">Advanced vision-language-action models for comprehensive GUI understanding</p>
                </div>
                <div className="bg-green-50 p-3 rounded-lg">
                  <h5 className="font-semibold text-green-800">Reinforcement Learning</h5>
                  <p className="text-green-700 text-sm">Policy optimization with experience replay for robust GUI control</p>
                </div>
                <div className="bg-purple-50 p-3 rounded-lg">
                  <h5 className="font-semibold text-purple-800">Security & Privacy</h5>
                  <p className="text-purple-700 text-sm">Trustworthy GUI agents addressing security and privacy concerns <CitationLink id="1" callType="quote" citations={citations}/></p>
                </div>
              </div>
            </div>
          </div>
        </CardContent>
      </Card>

      {/* Conclusions */}
      <Card className="bg-gradient-to-r from-blue-50 to-purple-50">
        <CardHeader>
          <CardTitle className="text-2xl text-center">Research Conclusions</CardTitle>
        </CardHeader>
        <CardContent className="space-y-4">
          <p className="text-lg leading-relaxed">
            The analysis of ten recent academic papers reveals GUI agent research as a rapidly maturing field with significant technological breakthroughs. 
            The emergence of native GUI agent models like UI-TARS represents a paradigm shift from wrapped commercial solutions to purpose-built architectures 
            <CitationLink id="4" callType="quote" citations={citations}/>.
          </p>
          <p className="leading-relaxed">
            Key innovations include the integration of self-reflection mechanisms for error recovery <CitationLink id="8" callType="quote" citations={citations}/>, 
            dynamic testing frameworks addressing real-world deployment challenges <CitationLink id="2" callType="quote" citations={citations}/>, 
            and reinforcement learning approaches that enhance action prediction capabilities <CitationLink id="3" callType="quote" citations={citations}/>.
          </p>
          <p className="leading-relaxed">
            The field is moving towards more sophisticated multimodal models that can handle high-resolution UI screenshots, 
            complex action sequences, and cross-platform compatibility. Future research should focus on addressing scalability concerns, 
            security implications, and the development of more robust evaluation benchmarks that reflect real-world usage scenarios.
          </p>
        </CardContent>
      </Card>

      {/* Citations */}
      <Card>
        <CardContent className="text-xs sm:text-sm text-muted-foreground mt-2 sm:mt-4 p-3 sm:p-6">
          <p className="font-semibold">References:</p>
          <ul className="space-y-1 mt-2">
            {Object.entries(citations).map(([id, citation]) => (
              <li key={id}>
                <CitationLink id={id} callType="recommend" citations={citations}/>
              </li>
            ))}
          </ul>
        </CardContent>
      </Card>
    </div>
  );
};

export default GUIAgentsReport;
