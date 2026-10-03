# Documentation search

## Documentation Domain Knowledge
You can search and read the official platform documentation dynamically.

### Documentation Directory Structure
Format: - {relative_path}: {page_title}

* /
  - /: /
* /changelog
  - /changelog: Product Changelog
  - /changelog/year-2023: 2023 Changelog
  - /changelog/year-2024: 2024 Changelog
  - /changelog/year-2025: 2025 Changelog
  - /changelog/year-2026: 2026 Changelog
* /docs
  - /docs: Introduction
  - /docs/actions: Action
  - /docs/actions/concept/interaction_model: Interaction Model
  - /docs/actions/guide/ai_integration: Build AI Agents
  - /docs/actions/guide/api_integration: Integrate APIs
  - /docs/actions/guide/building_action_flows: Build Actionflows
  - /docs/actions/guide/frontend_interactions: Build Frontend Interactions
  - /docs/actions/guide/payment/payment_airwallex: Airwallex Payment
  - /docs/actions/guide/payment/payment_overview: Payment Overview
  - /docs/actions/guide/payment/payment_stripe: Stripe Payment
  - /docs/actions/guide/sso_configuration: Use third-party sign-in
  - /docs/actions/reference: Reference
  - /docs/actions/reference/actionflow_node_list: Actionflow Node List
  - /docs/actions/reference/app_page: Client and Page Actions
  - /docs/actions/reference/component_operations: Component Actions
  - /docs/actions/reference/condition: Conditional
  - /docs/actions/reference/database_operation: Database Operations
  - /docs/actions/reference/for_each: Loop
  - /docs/actions/reference/navigation: Navigation Actions
  - /docs/actions/reference/show_toast: Toast and Modal
  - /docs/actions/reference/system: System
  - /docs/actions/reference/trigger_list: Triggers
  - /docs/actions/reference/user_event_collection: User Actions
  - /docs/billing: Billing
  - /docs/billing/commission_rule: Promoter Program
  - /docs/billing/my_wallet: My Wallet
  - /docs/billing/resource_management: Manage Project Resources
  - /docs/billing/upgrade_plan: Upgrade a Project
  - /docs/build_with_ai: Build with AI
  - /docs/build_with_ai/ai_copilot: Build with AI Copilot
  - /docs/build_with_ai/reading_what_ai_built: Read what AI built
  - /docs/build_with_ai/taking_over: Take over at any point
  - /docs/code: Extend with Code
  - /docs/code/code_component: Build Code Components
  - /docs/code/headless: Headless · Momen BaaS
  - /docs/code/run_code: Develop with Run Code
  - /docs/code/runtime_api: Runtime API Reference
  - /docs/data: Data
  - /docs/data/concept/data_flow: How Data Flows
  - /docs/data/concept/relational_data_modeling: Relational Data Modeling
  - /docs/data/concept/variables_and_parameters: Variables and Parameters
  - /docs/data/guide/bird_eye_view: Trace Data Dependencies
  - /docs/data/guide/conditional_data: Configure Conditional Data
  - /docs/data/guide/data_management: Manage Data Records
  - /docs/data/guide/database_configuration: Set Up the Database
  - /docs/data/guide/databinding_and_query: Query and Bind Data
  - /docs/data/guide/import_and_export: Import and Export Data
  - /docs/data/guide/resource_manager: Manage Uploaded Assets
  - /docs/data/guide/secret_management: Manage Secrets
  - /docs/data/guide/variable_parameter_usage: Use Page Variables and Parameters
  - /docs/data/guide/vector_data: Set Up Vector Search
  - /docs/data/reference/data_types: Data Types
  - /docs/data/reference/empty_values: Empty Values
  - /docs/data/reference/formula_dictionary: Formula Reference
  - /docs/design: Build UI
  - /docs/design/concept/layout_concepts: Layout System
  - /docs/design/concept/ui_organization_model: UI Organization
  - /docs/design/guide/studio_basics: Build Your First Page
  - /docs/design/guide/using_custom_components: Use Custom Components
  - /docs/design/reference/breakpoints: Breakpoints
  - /docs/design/reference/design_panel: Design Panel
  - /docs/design/reference/display_components: Display Components
  - /docs/design/reference/input_components: Input Components
  - /docs/design/reference/layout_components: Layout Components
  - /docs/design/reference/other_components: Other Components
  - /docs/design/reference/page_modal: Pages & Modals
  - /docs/design/reference/ui_shortcuts: UI Shortcuts
  - /docs/production: Publish & Operate
  - /docs/production/app_deployment: Publish an App
  - /docs/production/custom_domain: Use a Custom Domain
  - /docs/production/log_service: View Runtime Logs
  - /docs/production/mirror: Mirror (Real-Time Preview)
  - /docs/production/multiple_frontends: Build a Multi-Client App
  - /docs/production/permissions: Manage Permissions
  - /docs/production/reference/error_dictionary: Error Reference
  - /docs/production/reference/rendering_modes: Rendering Modes
  - /docs/production/seo: SEO for Web Apps
  - /docs/production/troubleshooting_guide: Troubleshoot an App
  - /docs/projects: Projects
  - /docs/projects/collaboration: Project Collaboration
  - /docs/projects/lifecycle: Project Management
  - /docs/start/bring_your_own_agent: Bring your own AI coding tool
  - /docs/start/editor_overview: Editor Overview
  - /docs/start/glossary: The Glossary
  - /docs/start/hello_world: 5-Minute Tutorial: To-Do List
  - /docs/start/mental_models: Mental Models
  - /docs/start/methodology: Methodology
* /templates
  - /templates: Template Center
  - /templates/content/blog_template_tutorial: Blog Template Guide
  - /templates/content/knowledge_paid_course_template_tutorial: Online Courses: A Nod to Udemy
  - /templates/content/smart_knowledge_base_tutorial: AI Knowledge Base
  - /templates/services/ai_mental_health_assistant_template_tutorial: AI Mental Health Assistant
  - /templates/services/mobile_auto_repair_template_tutorial: Mobile Auto Repair AI Scheduler
  - /templates/services/portfolio_template_tutorial: Portfolio
  - /templates/services/saas_corporate_site_template_tutorial: SaaS Corporate Site
  - /templates/utilities/ai_assistant_template_tutorial: AI Help Center
  - /templates/utilities/ai_feedback_tool_template_tutorial: AI Feedback Tool
  - /templates/utilities/angry_dietitian_template_tutorial: Angry Dietitian
  - /templates/utilities/feedback_tool_template_tutorial: Feedback Tool: A Nod to Canny
* /tutorial
  - /tutorial: Tutorials
  - /tutorial/ai_applications/ai_content_classifier: How to Build an AI Content Classifier?
  - /tutorial/ai_applications/ai_copy_reviewer: How to Build an AI Copy Reviewer?
  - /tutorial/ai_applications/ai_faq_matching: AI FAQ Matching
  - /tutorial/ai_applications/ai_image_describer: Automated Image Description Workflow
  - /tutorial/ai_applications/ai_needs_analysis: AI Needs Analyzer
  - /tutorial/ai_applications/ai_predictive_text_input: How to Implement AI Text Completion?
  - /tutorial/ai_applications/ai_product_image_generation: How to Build an AI Product Image Generator?
  - /tutorial/ai_applications/ai_resume_parser: How to Build an AI Resume Parser?
  - /tutorial/ai_applications/ai_smart_tagger: How to Build an AI Smart Tagger?
  - /tutorial/ai_applications/ai_spam_detector: How to Build an AI Spam Detector?
  - /tutorial/ai_applications/ai_summarizer_and_translator: How to Build an AI Summarizer & Translator
  - /tutorial/automations/auto_downgrade_membership: Automatic Membership Downgrade
  - /tutorial/automations/order_status_auto_updater: Order Status Auto Updater
  - /tutorial/feature_modules/cms_mvp: CMS (MVP Version)
  - /tutorial/feature_modules/daily_claim_limits: Daily Claim Limits (Two Approaches)
  - /tutorial/feature_modules/daily_claim_limits/state_counter: Daily Claim Limit (State Counter)
  - /tutorial/feature_modules/daily_claim_limits/unique_constraint: Daily Claim Limit (Unique Constraint)
  - /tutorial/feature_modules/form_drafts: Form Drafts (Two Approaches)
  - /tutorial/feature_modules/form_drafts/manual_save: Manual Draft Saving for Forms
  - /tutorial/feature_modules/form_drafts/realtime_save: Real-time Draft Saving for Forms
  - /tutorial/feature_modules/implement_referral_code_generation_and_attribution: Referral Code System
  - /tutorial/feature_modules/inventory_deduction: How to Build an Oversell-Proof Inventory Deduction System
  - /tutorial/feature_modules/login_register: Login Page Design
  - /tutorial/feature_modules/native_mobile_conversion: Momen App to Native Mobile Conversion
  - /tutorial/feature_modules/nested_list_seat_booking: How to Build a Nested List Seat Booking?
  - /tutorial/feature_modules/password_strength_validation: How to Implement Password Strength Validation
  - /tutorial/feature_modules/role_based_content_gating: How to Gate Content by User Role
  - /tutorial/feature_modules/web_verification_code_countdown: Verification code Countdown Timer

### Guidelines
1. When the user asks "how-to" questions, or requests guides, call docs.search first.
2. If a relevant page path is clearly visible in the directory structure above, you may directly call docs.get_page with that path without searching first.
3. Call docs.get_page to read the actual page content to extract accurate steps and facts.
4. Ground all explanations in the loaded documentation. Do not guess or fabricate.

## How to drive it (CLI only)

Read-only — no schema session needed.

```bash
npx -y momen-mcp@2.7.11 docs search --query "how to configure stripe payments"
npx -y momen-mcp@2.7.11 docs get-page --path "/03_data/01_database_basics"
```

`docs search` returns `{ path, title, url }` ranked by relevance; `url` is a public HTTPS link you can cite. Pass a returned `path` to `docs get-page` to read the full markdown. Search before answering how-to questions, and ground every claim in the retrieved page.
