# Novia Agent Configuration Reference

**Complete Agent Specifications for UI Design Creative Workflow**

---

## 1. Research Agent Cluster Configuration

### 1.1 Market Research Agent

```yaml
agent:
  id: "market_research_agent"
  name: "Market Research & Trend Analysis"
  type: "research_agent"
  
  model:
    provider: "anthropic"
    model_id: "claude-3.5-sonnet"
    temperature: 0.7
    max_tokens: 4000
    context_window: 200000
  
  system_prompt: |
    You are a market research specialist focused on identifying design trends, 
    competitive positioning, and emerging UI/UX patterns. Your research must be 
    current (2025 trends), comprehensive, and actionable for creative teams.
    
    Your responsibilities:
    1. Identify current design trends in the target industry
    2. Analyze competitor websites and applications
    3. Synthesize findings into actionable insights
    4. Provide recommendations for positioning
    
    Output format: Structured research report with citations and sources.
  
  tools:
    - semantic_search:
        provider: "pinecone"
        index: "design_trends_2025"
        top_k: 15
        
    - web_search:
        provider: "tavily_api"
        max_results: 20
        search_depth: "advanced"
        
    - trend_analysis:
        provider: "custom_module"
        lookback_days: 90
        
  memory:
    type: "semantic_memory"
    provider: "pinecone"
    embedding_model: "text-embedding-3-large"
    namespace: "research_context"
    retention: "session"
  
  expected_output:
    format: "markdown_report"
    sections:
      - "Executive Summary"
      - "Current Design Trends (5-10 trends)"
      - "Competitive Landscape"
      - "Positioning Recommendations"
      - "References"
    min_length: 1500
    max_length: 5000
```

### 1.2 User Persona Agent

```yaml
agent:
  id: "user_persona_agent"
  name: "User Insights & Persona Generation"
  type: "analysis_agent"
  
  model:
    provider: "openai"
    model_id: "gpt-4o"
    temperature: 0.6
    max_tokens: 3000
  
  system_prompt: |
    You are a UX research specialist creating detailed user personas. 
    Your personas must be based on data-driven insights and include:
    
    - Demographic information
    - Behavioral patterns
    - Pain points and goals
    - Device/platform usage
    - Accessibility needs
    - Technical proficiency
    
    Create 3-5 distinct personas that represent your target audience diversity.
  
  tools:
    - data_aggregation:
        sources:
          - "analytics_data"
          - "survey_responses"
          - "user_interview_transcripts"
          
    - pattern_recognition:
        algorithm: "clustering"
        dimensions:
          - "demographics"
          - "behavior"
          - "technical_level"
          
  memory:
    type: "vector_store"
    provider: "qdrant"
    namespace: "personas"
    
  expected_output:
    format: "structured_json_with_markdown"
    personas_count: 5
    fields_per_persona:
      - "name"
      - "demographics"
      - "bio"
      - "goals"
      - "pain_points"
      - "tech_proficiency"
      - "devices"
      - "accessibility_needs"
```

### 1.3 Competitor Analysis Agent

```yaml
agent:
  id: "competitor_analysis_agent"
  name: "Visual Competitor Analysis"
  type: "analysis_agent"
  
  model:
    provider: "anthropic"
    model_id: "claude-3.5-sonnet-vision"
    temperature: 0.5
    max_tokens: 3500
  
  tools:
    - web_scraper:
        targets:
          - "competitor_websites"
          - "design_showcase_platforms"
        output_format: "screenshots + structured_data"
        
    - visual_analysis:
        dimensions:
          - "layout_patterns"
          - "color_schemes"
          - "typography"
          - "component_usage"
          - "animation_patterns"
          
    - semantic_search:
        query_type: "visual_similarity"
        
  memory:
    type: "hybrid"
    components:
      - "semantic_memory"  # For text insights
      - "artifact_cache"   # For screenshots
  
  expected_output:
    format: "competitive_analysis_report"
    includes:
      - "Visual benchmarks (5-10 competitors)"
      - "Common design patterns"
      - "Differentiation opportunities"
      - "Design trend analysis"
```

---

## 2. Conceptualization Agent Cluster

### 2.1 Creative Director Agent

```yaml
agent:
  id: "creative_director_agent"
  name: "Creative Direction & Brand Voice"
  type: "creative_agent"
  
  model:
    provider: "anthropic"
    model_id: "claude-3.5-sonnet"
    temperature: 0.8  # Higher for creative exploration
    max_tokens: 4000
  
  system_prompt: |
    You are a creative director synthesizing brand strategy into visual direction.
    
    Your role:
    1. Analyze brand guidelines and business objectives
    2. Define visual aesthetic and mood
    3. Specify design language and principles
    4. Create mood boards and visual references
    5. Generate structured creative briefs for image generation
    
    Outputs must be specific, implementable, and aligned with business goals.
  
  tools:
    - mood_board_aggregation:
        sources:
          - "pinterest_api"
          - "dribbble_api"
          - "behance_api"
        count: 30
        
    - brand_analysis:
        input: "brand_guidelines"
        output: "strategic_insights"
        
    - prompt_templating:
        format: "structured_generation_prompts"
        
  memory:
    type: "semantic_memory"
    provider: "chroma"
    namespace: "creative_context"
  
  expected_output:
    format: "creative_brief_document"
    sections:
      - "Aesthetic Direction (200-300 words)"
      - "Visual Language"
      - "Color Philosophy"
      - "Typography Principles"
      - "Mood & Tone"
      - "Reference Imagery"
      - "Generation Prompt Templates"
```

### 2.2 Prompt Engineer Agent

```yaml
agent:
  id: "prompt_engineer_agent"
  name: "Structured Prompt Engineering"
  type: "engineering_agent"
  
  model:
    provider: "openai"
    model_id: "gpt-4o"
    temperature: 0.5  # Precision focus
    max_tokens: 2000
  
  system_prompt: |
    You are an expert in prompt engineering for image generation models.
    Your task is to translate creative briefs into optimized prompts that:
    
    1. Maximize semantic alignment with creative direction
    2. Specify technical parameters (composition, lighting, style)
    3. Provide fallback variations (3-5 prompt variants per asset)
    4. Account for model-specific capabilities and limitations
    
    Prompts must be:
    - Specific and descriptive
    - Grammatically correct
    - Free of ambiguous instructions
    - Include success criteria
  
  tools:
    - prompt_optimization:
        techniques:
          - "semantic_enrichment"
          - "composition_specification"
          - "style_anchoring"
          
    - model_capability_analyzer:
        models:
          - "dall-e-3"
          - "stable-diffusion-3"
          - "midjourney-v6"
          - "gemini-2.0-ultra"
        
    - prompt_testing:
        test_count: 2  # Quick validation
        
  memory:
    type: "semantic_memory"
    namespace: "prompt_library"
  
  expected_output:
    format: "prompt_specification_document"
    prompts_per_asset: 3
    fields:
      - "primary_prompt"
      - "backup_prompts"
      - "parameters" (model-specific)
      - "success_criteria"
      - "style_references"
```

### 2.3 Design System Architect Agent

```yaml
agent:
  id: "design_system_architect_agent"
  name: "Design System & Component Library"
  type: "architecture_agent"
  
  model:
    provider: "anthropic"
    model_id: "claude-3.5-sonnet"
    temperature: 0.4  # Lower for consistency
    max_tokens: 3500
  
  system_prompt: |
    You are a design systems architect creating comprehensive, scalable systems.
    
    Your responsibilities:
    1. Define spacing scales and rhythm
    2. Create color systems (tokens, scales)
    3. Specify typography system
    4. Document component patterns
    5. Generate design tokens (JSON/YAML)
    6. Ensure accessibility compliance
    
    Your design system must:
    - Be composable and maintainable
    - Support light AND dark modes
    - Comply with WCAG 2.1 AA standards
    - Include all component states
  
  tools:
    - design_token_generator:
        formats:
          - "json"
          - "yaml"
          - "css"
          - "scss"
          
    - component_library_builder:
        components:
          - "buttons"
          - "forms"
          - "cards"
          - "navigation"
          - "typography"
          - "spacers"
          
    - accessibility_validator:
        standards: "wcag_21_aa"
        checks:
          - "color_contrast"
          - "focus_states"
          - "semantic_html"
          
  memory:
    type: "artifact_cache"
    outputs:
      - "design_tokens.json"
      - "component_definitions.yaml"
      - "css_variables.css"
  
  expected_output:
    format: "design_system_specification"
    deliverables:
      - "Design tokens (JSON)"
      - "Component library (documented)"
      - "Color scales"
      - "Typography definitions"
      - "Spacing system"
      - "Accessibility guidelines"
```

---

## 3. Generation Agent Cluster

### 3.1 Image Generation Executor

```yaml
agent:
  id: "image_generation_executor"
  name: "Multi-Model Image Generation"
  type: "generation_agent"
  
  model_router:
    default: "dall-e-3"
    
    models:
      dall-e-3:
        provider: "openai"
        api_key: "${OPENAI_API_KEY}"
        quality: "hd"
        size: "1024x1024"
        n_images: 1
        
      stable-diffusion-3:
        provider: "stability-ai"
        api_key: "${STABILITY_API_KEY}"
        steps: 50
        cfg_scale: 7.5
        
      midjourney:
        provider: "midjourney_webhook"
        webhook_url: "${MIDJOURNEY_WEBHOOK}"
        quality: "2"  # --quality 2
        
      gemini-2.0-ultra:
        provider: "google"
        api_key: "${GOOGLE_API_KEY}"
        
  routing_strategy:
    default_selection: "prompt_based"
    rules:
      - condition: "asset_type == 'photo_realistic'"
        route_to: "dall-e-3"
        
      - condition: "asset_type == 'stylized_illustration'"
        route_to: "midjourney"
        
      - condition: "asset_type == 'abstract_concept'"
        route_to: "stable-diffusion-3"
        
      - condition: "complexity_score > 8"
        route_to: "gemini-2.0-ultra"
  
  execution:
    parallel_batch_size: 5
    timeout_per_image: 120
    retry_policy:
      max_retries: 2
      backoff_factor: 2
      
  output:
    format: "png"
    resolution: "1024x1024"  # Standard
    metadata:
      - "model_used"
      - "prompt"
      - "generation_time"
      - "quality_score"
```

### 3.2 Quality Validator Agent

```yaml
agent:
  id: "quality_validator_agent"
  name: "Image Quality & Compliance Validation"
  type: "validation_agent"
  
  model:
    provider: "openai"
    model_id: "gpt-4-vision"
    temperature: 0.3
    max_tokens: 1000
  
  system_prompt: |
    You are a rigorous quality validator for generated images.
    
    Your assessments cover:
    1. Technical Quality (sharpness, lighting, composition)
    2. Brand Alignment (matches creative direction)
    3. Semantic Accuracy (prompt alignment)
    4. Accessibility (text readability, contrast)
    5. Compliance (no inappropriate content)
    
    Score each dimension 0-100 and provide detailed feedback.
  
  validation_metrics:
    technical_quality:
      - "focus_sharpness"
      - "lighting_quality"
      - "color_accuracy"
      - "composition_balance"
      
    brand_alignment:
      - "style_consistency"
      - "color_palette_match"
      - "brand_essence"
      
    semantic_accuracy:
      - "prompt_adherence"
      - "subject_quality"
      - "detail_level"
      
    accessibility:
      - "text_readability"
      - "contrast_ratio"
      - "visual_clarity"
      
    compliance:
      - "content_safety"
      - "no_watermarks"
      - "no_artifacts"
  
  threshold_config:
    minimum_quality_score: 70
    minimum_brand_alignment: 75
    minimum_semantic_accuracy: 80
    
  auto_rejection_criteria:
    - "quality_score < 60"
    - "compliance_score < 50"
    - "error_artifacts_detected"
  
  expected_output:
    format: "validation_report"
    fields:
      - "overall_score" (0-100)
      - "dimension_scores"
      - "pass_fail_status"
      - "feedback"
      - "recommendations"
```

---

## 4. Refinement Agent Cluster

### 4.1 Art Director Agent

```yaml
agent:
  id: "art_director_agent"
  name: "Art Direction & Feedback Integration"
  type: "creative_feedback_agent"
  
  model:
    provider: "anthropic"
    model_id: "claude-3.5-sonnet-vision"
    temperature: 0.7
    max_tokens: 2000
  
  system_prompt: |
    You are an experienced art director synthesizing stakeholder feedback.
    
    Your role:
    1. Analyze user/stakeholder feedback on generated images
    2. Identify key refinement themes
    3. Generate specific, actionable prompt modifications
    4. Create variation instructions for generation agents
    5. Explain refinement rationale
    
    Your modifications must be:
    - Specific (not vague)
    - Actionable for prompt engineers
    - Preserve brand alignment
    - Incrementally improve quality
  
  tools:
    - feedback_analyzer:
        input: "user_feedback_text + generated_images"
        output: "structured_feedback_themes"
        
    - prompt_modifier:
        input: "original_prompt + feedback"
        output: "refined_prompts"
        techniques:
          - "parameter_adjustment"
          - "style_refinement"
          - "composition_modification"
          
    - variation_generator:
        count: 3
        
  memory:
    type: "semantic_memory"
    namespace: "feedback_context"
  
  feedback_categories:
    - "composition" # Re-frame, adjust perspective
    - "color" # Adjust palette
    - "style" # Shift artistic direction
    - "detail" # Add/remove elements
    - "lighting" # Modify illumination
    - "mood" # Emotional adjustment
```

### 4.2 Consensus Builder Agent

```yaml
agent:
  id: "consensus_builder_agent"
  name: "Stakeholder Alignment & Consensus"
  type: "coordination_agent"
  
  model:
    provider: "gpt-4o"
    temperature: 0.5
    max_tokens: 1500
  
  system_prompt: |
    You are a skilled facilitator building consensus among diverse stakeholders.
    
    Your responsibilities:
    1. Synthesize conflicting feedback
    2. Identify common ground
    3. Escalate unresolvable conflicts
    4. Create decision matrices
    5. Document final selections with justification
    
    Balance creative vision with practical constraints.
  
  tools:
    - feedback_synthesis:
        input: "all_stakeholder_feedback"
        analysis:
          - "sentiment_analysis"
          - "priority_weighting"
          - "conflict_identification"
          
    - decision_matrix_builder:
        criteria:
          - "brand_alignment"
          - "user_appeal"
          - "technical_quality"
          - "implementation_feasibility"
        weight_strategy: "stakeholder_input"
        
    - reporting:
        output_format: "consensus_report"
        includes:
          - "selected_assets"
          - "decision_rationale"
          - "stakeholder_alignment"
          - "next_steps"
```

---

## 5. Monitoring & Dashboard Configuration

### 5.1 Metrics Collector

```yaml
monitoring:
  metrics_collection:
    interval: "5_seconds"
    batch_size: 100
    storage: "prometheus"
    
  collected_metrics:
    phase_metrics:
      - name: "phase_execution_time"
        unit: "seconds"
        type: "gauge"
        
      - name: "phase_success_rate"
        unit: "percentage"
        type: "gauge"
        
    agent_metrics:
      - name: "agent_execution_time"
        unit: "milliseconds"
        type: "histogram"
        percentiles: [50, 95, 99]
        
      - name: "agent_queue_depth"
        unit: "count"
        type: "gauge"
    
    quality_metrics:
      - name: "average_quality_score"
        unit: "0-100"
        type: "gauge"
        
      - name: "quality_pass_rate"
        unit: "percentage"
        type: "gauge"

  dashboards:
    - name: "workflow_overview"
      widgets:
        - "phase_timeline_chart"
        - "success_rate_gauge"
        - "cost_tracker"
        
    - name: "agent_health"
      widgets:
        - "agent_status_grid"
        - "latency_distribution"
        - "error_rates"
```

### 5.2 Alert Rules

```yaml
alerts:
  phase_alerts:
    - alert_id: "phase_timeout"
      condition: "phase_execution_seconds > 3600"
      severity: "critical"
      action: "pause_workflow + notify_operators"
      
  quality_alerts:
    - alert_id: "low_quality_batch"
      condition: "batch_average_quality_score < 70"
      severity: "high"
      action: "halt_generation + investigate"
      
  cost_alerts:
    - alert_id: "budget_warning"
      condition: "cumulative_cost > budget * 0.80"
      severity: "medium"
```

---

## 6. Workflow Orchestration Schema

```yaml
workflow:
  name: "ui_design_creative_pipeline_v1"
  version: "1.0"
  
  phases:
    - phase_number: 1
      phase_name: "research_cadrage"
      execution_mode: "parallel"
      agents:
        - market_research_agent
        - user_persona_agent
        - competitor_analysis_agent
      timeout: 1800
      
    - phase_number: 2
      phase_name: "conceptualization"
      execution_mode: "sequential"
      agents:
        - creative_director_agent
        - prompt_engineer_agent
        - design_system_architect_agent
      dependencies:
        - "phase_1"
      timeout: 1200
      
    - phase_number: 3
      phase_name: "generation"
      execution_mode: "parallel"
      agents:
        - image_generation_executor
      dependencies:
        - "phase_2"
      scaling:
        type: "horizontal"
        min_instances: 2
        max_instances: 10
      
    - phase_number: 4
      phase_name: "refinement"
      execution_mode: "sequential_with_loops"
      agents:
        - quality_validator_agent
        - art_director_agent
        - consensus_builder_agent
      dependencies:
        - "phase_3"
      feedback_loop_capacity: 3
  
  state_management:
    backend: "redis"
    context_store: "semantic_memory"
    artifact_storage: "s3"
```

---

## 7. Implementation Commands

```bash
# Deploy research cluster
novia deploy agents \
  --agents market_research_agent,user_persona_agent,competitor_analysis_agent \
  --cluster research \
  --auto-scale true

# Deploy generation cluster
novia deploy agents \
  --agents image_generation_executor \
  --cluster generation \
  --scaling horizontal:2:10

# Execute workflow
novia execute workflow \
  --workflow-name ui_design_creative_pipeline_v1 \
  --project-brief ./project_brief.json \
  --monitor dashboard
```

---

**End of Configuration Reference**

**Document Version:** 1.0  
**Last Updated:** December 19, 2025  
**Total Lines:** 940  
**Status:** Production-Ready YAML Configurations