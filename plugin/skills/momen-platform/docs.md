# Documentation search

## Documentation Domain Knowledge
You can search and read the official platform documentation dynamically.

### Documentation Directory Structure
Format: - {relative_path}: {page_title}

* /
  - /: Introduction & Pathways
* /account_community
  - /account_community: Account & Community
  - /account_community/collaboration: Collaboration
  - /account_community/commission: Promoting Momen
  - /account_community/headless_vibe_coding: Connect to Momen Backend (Using Momen BaaS)
  - /account_community/my_wallet: My Wallet
* /actions
  - /actions: Actions
  - /actions/actionflow/custom_code: Custom Code
  - /actions/actionflow/overview: Actionflow
  - /actions/ai/ai_data_model: AI Data Model
  - /actions/ai/ai_point: AI Points
  - /actions/ai/overview: AI Agent
  - /actions/ai/vector_storage: Vector Data Storage and Sorting
  - /actions/api: API Integration
  - /actions/app_page_management: App and Page Management
  - /actions/basic_operation: Basic Operations of Action Configuration
  - /actions/clipboard: Clipboard
  - /actions/component_management: Component Management
  - /actions/condition: Conditional
  - /actions/file_management: File Management
  - /actions/for_each: For Each
  - /actions/interaction_model: Interaction Model
  - /actions/location: Location
  - /actions/navigation: Navigation
  - /actions/payment/payment_airwallex: Momen Payment Configuration: Airwallex Integration and Optimization Guide
  - /actions/payment/payment_overview: Momen Payment Overview
  - /actions/payment/payment_stripe: Stripe Payment
  - /actions/qr_code: QR Code
  - /actions/request: Request
  - /actions/share: Share
  - /actions/sso: SSO
  - /actions/toast_modal: Toast Notifications & Modal
  - /actions/trigger_list: Trigger Reference
  - /actions/user_actions: User Actions
* /changelog
  - /changelog: Latest Product Update
* /data
  - /data: Data Processing
  - /data/bird_eye_view: Bird's-eye View: Visualize Project Data Relationships
  - /data/database/configuration: Momen Data Model and Database Complete Guide: Structured Modeling and Efficient Management
  - /data/database/data_management: Data Management Guide
  - /data/database/import_and_export: Momen Data Import and Export - Import Excel, CSV, and Multimedia Files with Data Update and Conflict Resolution
  - /data/databinding: Momen Data Usage Complete Guide: CRUD Operations and Practical Case Studies
  - /data/formula: Formula and Conditions
  - /data/parameter: Parameter
  - /data/resource_manager: Resource Manager
  - /data/secret: Momen Secret Management
  - /data/variable: Variable
* /debugging
  - /debugging/component_system_refactor: Component System Upgrade and Migration Guide
  - /debugging/how_to_debug_in_momen: How to Debug in Momen
  - /debugging/request_error_reference: Request Error Reference
* /deployment
  - /deployment: Release & Growth
  - /deployment/hosting_file: Hosting Files
  - /deployment/log_service: Log Service
  - /deployment/mirror: Mirror (Real-time Preview)
  - /deployment/multiple_clients: Multiple Clients
  - /deployment/permission: Permissions
  - /deployment/project_management: Project Management
  - /deployment/publish: Publish Application
  - /deployment/seo: SEO
  - /deployment/upgrade_project: Project Upgrade and Resource Management
* /design
  - /design: UI Building
  - /design/breakpoints: Breakpoints
  - /design/canvas: Canvas Operations
  - /design/code_component/cli_changelog: CLI Tool Changelog
  - /design/code_component/code_component_dev: Code Component Development
  - /design/code_component/component_api: Code Component API Reference
  - /design/components: Components
  - /design/conditional_view: Conditional View
  - /design/custom_component: Custom Component
  - /design/display: Components - Display
  - /design/input: Components - Input
  - /design/layout: Layout and Position
  - /design/list: List
  - /design/map: Map
  - /design/others: Components - Others
  - /design/pages: Add and Configure Pages
  - /design/select_view: Select View
  - /design/tab_view: Tab View
  - /design/view: View
* /help
  - /help/development: Documentation Development Guide
* /starts
  - /starts/editor_overview: Editor Overview
  - /starts/glossary: The Glossary
  - /starts/hello_world: 5-Minute Tutorial: To-Do List
  - /starts/mental_models: Mental Models
  - /starts/methodology: Methodology
* /template
  - /template: Template Center
  - /template/ai_feedback_tool: AI Feedback Tool
  - /template/ai_help_center: AI Help Center
  - /template/ai_knowledge_base: AI Knowledge Base
  - /template/ai_mental_health_assistant: AI Mental Health Assistant
  - /template/angry_dietitian: Angry Dietitian
  - /template/blog: Blog Template Guide
  - /template/feedback_tool_a_nod_to_canny: Feedback Tool: A Nod to Canny
  - /template/mobile_auto_repair_ai_scheduler: Mobile Auto Repair AI Scheduler
  - /template/online_courses_a_nod_to_udemy: Online Courses: A Nod to Udemy
  - /template/portfolio: Portfolio
  - /template/saas_corporate_site: SaaS Corporate Site
* /tutorial
  - /tutorial: Tutorials - Step-by-Step Guides for Momen Development
  - /tutorial/how_to_build_a_cms_mvp_version_in_hours: CMS (MVP Version)
  - /tutorial/how_to_build_a_nested_list_seat_booking: How to Build a Nested List Seat Booking?
  - /tutorial/how_to_build_a_referral_code_system: How to Implement Referral Code Generation and Verification?
  - /tutorial/how_to_build_an_ai_content_classifier: How to Build an AI Content Classifier?
  - /tutorial/how_to_build_an_ai_copy_reviewer: How to Build an AI Copy Reviewer?
  - /tutorial/how_to_build_an_ai_image_describer: Automated Image Description Workflow
  - /tutorial/how_to_build_an_ai_knowledge_base: AI Knowledge Base
  - /tutorial/how_to_build_an_ai_needs_analysis_project: AI Needs Analyzer
  - /tutorial/how_to_build_an_ai_powered_smart_tagger: How to Build an AI Smart Tagger?
  - /tutorial/how_to_build_an_ai_product_image_generator: How to Build an AI Product Image Generator?
  - /tutorial/how_to_build_an_ai_resume_parser: How to Build an AI Resume Parser?
  - /tutorial/how_to_build_an_ai_spam_detector: How to Build an AI Spam Detector?
  - /tutorial/how_to_build_an_automatic_membership_downgrade: Automatic Membership Downgrade
  - /tutorial/how_to_build_an_order_expiry_scheduler: How to Build an Order Status Auto-Updater？
  - /tutorial/how_to_convert_momen_app_to_native_mobile_app: Momen App to Native Mobile Conversion
  - /tutorial/how_to_design_your_login_page: Login Page Design
  - /tutorial/how_to_implement_ai_text_completion: How to Implement AI Text Completion?
  - /tutorial/how_to_set_a_countdown_timer_when_sending_a_verification_code: Verification Code Countdown Timer

### Guidelines
1. When the user asks "how-to" questions, or requests guides, call docs.search first.
2. If a relevant page path is clearly visible in the directory structure above, you may directly call docs.get_page with that path without searching first.
3. Call docs.get_page to read the actual page content to extract accurate steps and facts.
4. Ground all explanations in the loaded documentation. Do not guess or fabricate.

## How to drive it (CLI only)

Read-only — no schema session needed.

```bash
"${PLUGIN_ROOT:-${CLAUDE_PLUGIN_ROOT}}/bin/momen-mcp" docs search --query "how to configure stripe payments"
"${PLUGIN_ROOT:-${CLAUDE_PLUGIN_ROOT}}/bin/momen-mcp" docs get-page --path "/03_data/01_database_basics"
```

`docs search` returns `{ path, title, url }` ranked by relevance; `url` is a public HTTPS link you can cite. Pass a returned `path` to `docs get-page` to read the full markdown. Search before answering how-to questions, and ground every claim in the retrieved page.
