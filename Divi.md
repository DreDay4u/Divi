
## 1. Workflow Overview

- **End-to-End Pipeline:** The process begins with a human or system providing a detailed prompt describing the desired website (layout, style, content, features). The LLM processes this prompt and **outputs a JSON file** that encapsulates the entire Divi design (sections, rows, modules, settings, and even images). The JSON is structured according to Divi’s portability format so it can be directly imported into WordPress via the Divi Builder.
    
- **Manual Import & Deployment:** Given ease-of-use as a priority, the generated JSON is typically uploaded **manually through Divi’s Import interface** (Divi > Divi Library > Import & Export). This avoids complex setup and leverages Divi’s built-in portability system. Upon import, all layout content, design settings, and media assets contained in the JSON are automatically added to the site’s Divi Library or Theme Builder, ready for use.
    
- **Alternate Automation (Optional):** For advanced workflows, the JSON could also be imported using automation tools (e.g. a custom plugin or WP-CLI commands), but manual UI import is the simplest starting point. Similarly, media assets can be auto-uploaded via WordPress REST API calls if fully automating the pipeline, though initially the focus is on a mostly manual import for reliability.
    
- **Result:** The outcome is a fully designed website or page (or set of pages/templates) in WordPress, built with Divi modules and styles. Designers can then tweak the imported layouts visually using Divi Builder if needed. Because the JSON adheres to Divi’s format, it ensures **maximum compatibility across Divi sites and even Divi Cloud** storage for reuse.
    

## 2. LLM Prompt Structure

- **Contextual Instruction:** Begin the prompt by clearly instructing the LLM of its role and the required output format. For example: _“You are an expert web designer using Divi. Produce a **Divi layout JSON** that implements the following design… [describe design]. Only output valid JSON, no explanations.”_ This sets the context that the LLM should respond with structured JSON code only, suitable for Divi import.
    
- **Content Requirements:** In the user prompt, describe the desired **content and structure** of the site in detail. Include specifics like: number of sections, column layouts for each row, text for headings/paragraphs, button labels, images needed and their themes, etc. If a full site or multiple pages are needed, clarify each template (e.g. “homepage with hero, about section…; global header with logo and menu; global footer with signup form…”). The more precise the description, the more accurately the LLM can build the layout.
    
- **Design and Styling Instructions:** Include style guidelines such as color scheme, fonts/typography, spacing, and any specific design elements (e.g. “use a gradient background in the hero”, “apply a box shadow to images”, “use modern flat design icons”). Also mention responsive behavior if any particular tweaks are needed for mobile vs desktop. The prompt should encourage use of Divi’s design settings (instead of raw CSS whenever possible) so that the JSON leverages Divi’s built-in style options.
    
- **Module and Feature Hints:** Guide the LLM on which Divi modules or features to use for certain content. For example, if a blog feed is needed, mention the **Blog Module** for dynamic post listings; for contact forms, mention using a Code module to embed a form if a native module isn’t available. This helps the LLM choose the correct JSON structures (e.g. an `et_pb_blog` module for a blog listing). If dynamic content or Theme Builder conditions are needed, call that out (e.g. “use dynamic content for post titles and featured images in the blog template”).
    
- **Output Formatting Directions:** End the prompt by reiterating that the output must be **valid JSON** compatible with Divi. For instance: _“Output the complete Divi layout JSON with all necessary keys and nested structures, ready to import. Do not include any explanatory text.”_ This ensures the LLM knows to present just the JSON code. Optionally, specify that the JSON should be pretty-formatted for readability or compressed if file size is a concern (Divi accepts either as long as it’s valid JSON).
    

## 3. Divi JSON Schema Mapping

- **Hierarchy:** Divi’s JSON layout structure is hierarchical, mirroring the Divi Builder’s elements. At the top level, the JSON contains metadata (like `et_builder_version`) and an array of `sections`. Each **Section** object can be of types _regular_, _specialty_, or _fullwidth_, and contains one or more rows. Each **Row** in turn contains columns (the row’s column structure is often indicated by keys like `column_structure` or an array of column objects). Inside each column lies an array of **Module** objects, which are the actual content elements (text, images, sliders, etc.). This nesting must be preserved exactly for Divi to recognize the layout.
    
- **Module Objects:** Every Divi module in JSON has a specific signature. For example, a Text module appears with a module name like `"module": "Text"` or an identifier like `"et_pb_text"`, and includes content and design settings. A simplified example of a Text module in JSON might look like:
    
    ```json
    {
      "module": "Text",
      "content": "<h2>Welcome to Our Site</h2><p>This is some intro text.</p>",
      "settings": {
        "text_orientation": "center",
        "text_text_color": "#333333",
        "module_alignment": "center"
      }
    }
    ```
    
    In the above, `"content"` holds the HTML content for the module, and `"settings"` (or sometimes `"styles"`) holds design configuration (colors, alignment, etc.). Each module type (Image, Button, Gallery, Code, etc.) will have its own expected fields in JSON (e.g. Image modules include an `src` for the image URL or data, alt text, etc., while a Blurb might include title, icon, image). The LLM must insert the appropriate fields per module type.
    
- **Global Presets & Design Tokens:** Divi exports often include a section for global presets or design settings if those are used. This might appear as a `globalPresets` object mapping preset IDs to styles. For blueprint simplicity, the LLM can either use the default styles or define inline design settings on each module. However, for consistency and scalability, you may instruct the LLM to use a limited set of global presets (like a preset for all buttons, etc.) which would then be referenced by preset ID in module JSON. If used, these preset definitions would appear in the JSON so that Divi can import them as well.
    
- **Dynamic Content & Theme Builder Conditions:** To support dynamic data (e.g. blog post templates showing post title, featured image, etc.), Divi’s JSON can reference dynamic content placeholders. In practice, when designing in Divi, selecting dynamic content inserts a token (for example, a Post Title module or a Text module with dynamic content might have a flag in JSON). Ensure the LLM knows to include these when needed – e.g., a Post Title module rather than a static text for a template. Similarly, if generating Theme Builder templates, the JSON should include any **assignment conditions** (for instance, a template assigned to All Posts, or to a specific category). Divi’s Theme Builder JSON format includes a section listing which template (header/body/footer) and the conditions. While the exact schema for conditions is complex, the LLM can be guided to use known examples (e.g., a template object with `"assignedPosts": "all"` or category IDs). After import, users can verify or adjust conditions via the Divi Theme Builder UI.
    
- **Complete JSON Package:** The JSON should encapsulate all needed data for the layout. This includes textual content, design settings (margins, padding, colors, font sizes, etc.), and references to images or icons. **Everything is self-contained in one JSON file** for portability. By structuring the output precisely to match Divi’s expected keys (e.g., using `"sections"`, `"rows"`, `"columns"`, `"modules"`, and proper module identifiers), the LLM ensures the JSON can be imported without errors. It’s advisable to use an existing Divi export as a reference for key names and structure when fine-tuning the LLM’s output mapping.
    

## 4. Asset Integration

- **Image Placeholders and Prompts:** When the design calls for images (hero banners, gallery photos, icons, etc.), the LLM should allocate a module for them (e.g., an Image module or Gallery module) and can initially provide a **placeholder reference**. Since this system plans to use AI-generated images, the LLM could output an `"src"` field with a dummy image URL or base64 placeholder, along with an `"alt"` text. More helpfully, the LLM can produce an **image generation prompt** as a comment or separate field, describing the desired image (for example: `"alt": "Hero image of a modern office", "ai_image_prompt": "photo of modern startup office, people working..."`). This prompt can guide the image generation tool to create suitable visuals for that section.
    
- **Embedding Images in JSON:** Divi’s portability can include images directly. When exporting layouts, Divi encodes images such that importing will automatically upload them to the Media Library. In JSON, this typically appears as a data URI (base64-encoded image data) or a placeholder that the import process replaces with actual uploads. For automation, after obtaining AI-generated images, you have two main options:
    
    - _Manual Approach:_ Replace the placeholder src in the JSON with the actual image URL after you upload it to the Media Library (or use the Media Library ID if known). Then proceed to import. Divi will fetch and store it.
        
    - _Automated via API:_ Use WordPress REST API’s media endpoint (`POST /wp/v2/media`) to programmatically upload the image file, obtaining an ID or URL. Then inject that URL/ID into the JSON’s image module before import. This ensures the JSON references a valid image that will be imported.
        
- **Media Format & Optimization:** The blueprint emphasizes performance, so all images should be optimized **before or during upload**. Prefer modern formats like **WebP** for excellent compression, or JPEG at high quality settings for photographic content. Aim to keep image file sizes low (ideally under 200KB for large images, and under 80KB for smaller graphics) to ensure fast load times. The LLM’s image prompt suggestions can even include style cues that lead to simpler, compressible images (e.g. “minimalist illustration” vs. “photorealistic scene” depending on context). After generation, run the images through compression tools if needed.
    
- **In-JSON Asset Handling:** When the LLM outputs the JSON, it may include base64 data URIs for small assets (like inline SVGs or icon data). Ensure these are not overly large. For larger images, it might just reference a link (to be replaced by a real link). Divi’s import will handle bringing in external images if the JSON is structured correctly, or embed base64 directly if provided. Always double-check that the JSON **“img_src”** or equivalent fields either contain a valid data URI or a properly formatted URL; otherwise, the image module might import blank.
    
- **Alt Text and SEO:** The LLM should be instructed to always include descriptive **alt text** for images in the JSON. This improves accessibility and SEO. For example, if adding a hero banner image of a team, the alt could be “Team collaborating in a modern office”. Including these in the JSON ensures that once imported, the images on the site have appropriate alt attributes set. This aligns with best practices for web accessibility (WCAG) and on-page SEO.
    

## 5. Validation & Testing

- **JSON Syntax Check:** Before attempting any import, run the LLM’s output through a JSON linter or validator to catch syntax errors (missing commas, quotes, mismatched braces, etc.). The LLM might occasionally produce a format issue, so this step is critical. A simple way is to paste the JSON into a tool like JSONLint or use a command-line tool (`jq` or Node.js `JSON.parse`) to confirm it’s well-formed.
    
- **Schema Conformance:** Develop a lightweight **JSON schema or checklist** for Divi layouts to validate the presence of key elements. For example, verify that the top-level contains `"sections"` array, that each section has at least one row, each row has columns, and each column has modules. Also check that each module object has the expected fields (`module/type`, `content` or `options`, etc., depending on Divi’s format). This could be a custom script or just a manual review against a template. The goal is to ensure the structure matches what Divi expects – e.g., a missing `"columns"` wrapper or a misspelled key like `"moduls"` would cause the import to fail or be incomplete.
    
- **Trial Import in Sandbox:** It’s wise to test the JSON on a **staging WordPress site** with Divi before using it in production. Import the layout via Divi Library’s Import function. Divi will either import successfully or throw an error if something is significantly wrong (in which case, use the error message to guide fixes – e.g., if Divi says “Invalid Module type X”, adjust the JSON to use a correct module name). During trial import, verify that all content appears as intended: correct number of sections, text in place, images showing, and design settings applied.
    
- **Visual QA:** After import on the test site, **inspect the page in Divi Builder**. Check that responsive behavior is correct by toggling Divi’s mobile preview. Ensure modules stack or resize appropriately. If any design element looks off (for instance, a text color not applied), it might mean the JSON missed a setting or used an outdated property name. Tweak the JSON accordingly, or instruct the LLM with a refined prompt and regenerate, then test again. This iterative testing helps fine-tune the LLM’s output patterns.
    
- **CLI Diff Tool (Optional):** For developers, consider creating a small CLI tool that can compare the LLM-generated JSON to a “known good” Divi JSON file (perhaps a minimal layout) and flag differences in structure. This tool could, for example, ensure all module types in the JSON are from a whitelist of Divi modules, or that no required fields are null. It could also check the JSON against the Divi version (if the LLM includes `et_builder_version`, make sure it matches the version of Divi on the target site). Such pre-checks add confidence before import.
    
- **Error Handling:** If errors occur (e.g., JSON fails to import, or part of the layout is missing), log them and update the blueprint or prompt instructions. Common issues might include unsupported characters in text (which JSON might need escaped), too-large images causing import to time out, or modules not existing (if LLM invented a module name). Each time, refine the pipeline: e.g., instruct the LLM to use only known Divi modules and standard field names. Over time, these validations will make the generation more robust.
    

## 6. Deployment Steps

1. **Prepare WordPress Environment:** Ensure WordPress with the Divi Theme (or Divi Builder plugin) is installed and activated. Have the **Divi license key activated** on the site so that the import functionality is fully enabled. For full site templates, also make sure no conflicting theme builder templates exist (or be prepared to override them).
    
2. **Generate JSON via LLM:** Provide the carefully crafted prompt (as per section 2) to the LLM. Review the returned JSON for completeness. This is the design blueprint of the site. If multiple JSON files are needed (e.g., one for a header template, one for a footer, one for main pages), run separate prompts or have the LLM output multiple JSON objects with clear separators. Save each JSON output to a `.json` file with a descriptive name (e.g., `homepage-layout.json`, `global-header.json`).
    
3. **Incorporate Images:** For each image slot in the JSON, generate the actual image using an AI tool or stock resource, guided by the LLM’s prompt suggestions. Compress/convert the images to web-friendly formats and sizes (e.g., JPEG or WebP, max ~150KB). Then integrate the images into the JSON: if embedding, replace placeholder data with base64 strings; if linking, upload the image to the WordPress Media Library (manually or via REST API) and update the JSON’s `src` to the media file URL. Double-check that `"alt"` texts in JSON are set appropriately.
    
4. **Backup Current Site (if applicable):** If deploying to an existing site, back up the database and content. While importing Divi layouts is generally safe and doesn’t overwrite existing content unless you apply a template globally, it’s good practice to have backups in case you need to revert.
    
5. **Import via Divi Library:** In WordPress admin, navigate to **Divi > Divi Library**. Click “Import & Export” and choose the **Import** tab. Select the JSON file (for a single page or a library collection) and upload. Divi will import it; upon success, the new layout will appear in the Divi Library. If you are importing a Theme Builder pack (full site template JSON), go to **Divi > Theme Builder** and use the portability icon there to import the template pack (which can include header, footer, etc. in one file).
    
6. **Apply Layouts and Templates:** For page layouts, you can now create a new page (or open an existing one), enable Divi Builder, and load the imported layout from the Library. For global templates (headers/footers), verify in the Theme Builder interface that they are now listed. Assign any template conditions if the JSON didn’t include them – e.g., set the “All Posts” template to apply to all blog posts, etc., using Divi’s UI. This ensures dynamic templates take effect site-wide as intended.
    
7. **Post-Import Checks:** Visit the site pages to ensure everything looks correct. Check that the global header/footer appear everywhere expected, and that page content is intact. Also verify that interactive elements (forms, sliders, etc.) are functioning. Because we instructed the LLM to keep custom code inline and minimal, all functionality should be self-contained. If using third-party plugins (e.g., a Forminator form), you might need to edit the imported layout and re-add the form module or shortcode manually, since those might not export/import via Divi JSON.
    
8. **Optimization & Caching:** After deployment, enable any caching or optimization plugins as appropriate. Divi by default will regenerate static CSS files for the imported layouts (you can trigger this by saving the Theme Options, which clears and rebuilds Divi CSS cache). Ensure that minification/combine settings in Divi or plugins are turned on if desired, and that there are no console errors on the front-end. The site should now be live with the AI-generated design, and further manual edits can be done through Divi’s visual editor as needed.
    

## 7. Optimization & SEO Guidelines

- **Performance-First Design:** The LLM-generated layout should adhere to performance best practices. This means **avoiding overly complex or unnecessary modules** that bloat the DOM, keeping the number of sections and nested rows reasonable. Use Divi’s built-in options instead of heavy custom code where possible (e.g., use Divi’s animation and transition features rather than injecting large JS libraries). Divi is generally performant, but designs should still be kept lean. Remember that a strategically minimalist design not only loads faster but can improve SEO by focusing on content and structure rather than extraneous visuals.
    
- **Static CSS and Caching:** Divi automatically aggregates module styles into static CSS files for front-end performance. Ensure this feature is enabled (in Divi Theme Options > Performance). The blueprint’s emphasis on using Divi’s settings (as opposed to a lot of inline `<style>` tags) means we naturally leverage this static CSS generation. After importing a layout, **clear Divi’s cache** so that new static files are generated. Leverage browser caching by having proper headers (usually handled by plugins or server configs).
    
- **Responsive & Mobile-Friendly:** With nearly **60% of web traffic now coming from mobile devices**, mobile optimization is non-negotiable. The LLM’s output must include responsive design settings: use Divi’s responsive options for padding/margins, column stacking, and any element visibility toggles. For instance, if a desktop layout has a wide image, ensure it shrinks or swaps on mobile. Use percentage or relative units (em, rem) for text sizes where appropriate so text is readable on small screens. Also, test touch targets (buttons, sliders) for usability on mobile. If any sections are particularly heavy, consider using Divi’s visibility controls to disable or simplify them on mobile devices.
    
- **SEO Meta and Structure:** While Divi JSON covers the layout and content, remember to fill in SEO-relevant fields after import. Use proper HTML hierarchy in the content (e.g., the LLM can be instructed to use `<h1>` for the main title in a Text module, `<h2>` for section headings, etc., which Divi will preserve in the output HTML). This semantic structure helps search engines. Additionally, utilize Divi’s **SEO settings** or a plugin (RankMath, Yoast) to add meta titles and descriptions for each page. Divi’s Theme Options has fields for homepage SEO, canonical URLs, etc., which you should configure. The LLM won’t handle meta tags in the JSON (since that’s outside the layout), so make this part of your manual deployment checklist.
    
- **Schema Markup:** For advanced SEO, you might integrate schema markup for things like business info, FAQs, etc. This isn’t directly done through Divi’s layout JSON, but you can have the LLM include a Code module with the relevant JSON-LD script if a particular page calls for it. Otherwise, plan to add schema via a plugin or custom code after deployment.
    
- **Accessibility Considerations:** Even if formal WCAG compliance isn’t a current requirement, following basic accessibility is good practice. The LLM’s output should include alt text for images (as noted), and avoid illegible color contrasts (e.g., ensure text color vs background color has sufficient contrast). Use Divi’s accessibility enhancements like skip links or ARIA labels when appropriate (Divi has improved in accessibility in recent updates). Avoid content that flashes or animates in a way that could be problematic for users with disabilities.
    
- **Analytics & SEO Tracking:** Post-deployment, integrate Google Analytics or other tracking as needed, and use Google’s PageSpeed Insights or Lighthouse to audit the site. If the score is low due to, say, large images or un-minified CSS, address those (the blueprint already suggests image compression and Divi’s static CSS covers minification to an extent). The aim is a fast, mobile-friendly, and content-rich site which Divi can certainly deliver when used carefully. By combining Divi’s visual power with these optimization guidelines, the final websites should load quickly and rank well, without requiring extensive post-LLM fixes.
    

## 8. Example Prompts

To illustrate how one might prompt the LLM and what the output would look like, here are a few examples:

- **Example 1: Single Page Layout (Homepage)**  
    **User Prompt:** _“Design a modern homepage for a tech startup using Divi. It should have a hero section with a background image, an overlay title and subtitle (centered, white text). Below that, a three-column section showing the company’s services with icons and headings. Then a fullwidth testimonial slider, and a final contact form section. Use a blue and white color scheme with a clean, flat design. Output the layout as JSON ready for Divi import, including sample text and placeholder images (with prompts for AI image generation). No additional commentary, only JSON.”_  
    **Expected LLM Output:** The LLM will produce a JSON structure containing: one top-level section for the hero (with a background image URL or data, a Text module for title/subtitle, centered styling, likely using Divi’s overlay/text settings for hero), another section with a row of three columns each containing a Blurb module or Image+Text modules for services (with an icon or image placeholder in each, and a heading/text for each service), a fullwidth section or regular section for the testimonial slider (using Divi’s Testimonial module or Slider module configured with testimonial content), and a final section with a contact form (could use a Code module to embed a Forminator shortcode if that’s the plugin, or a Contact Form module if Divi’s built-in form is acceptable). The JSON will include all necessary content and design settings (e.g., the blue color applied to buttons or icons). For brevity, a snippet from the JSON might look like:
    
    ```json
    {
      "sections": [
        {
          "type": "regular",
          "settings": {
            "background_image": "https://example.com/ai-generated-office.jpg",
            "padding": "100px 0px 100px 0px"
          },
          "rows": [{
            "columns": [{
              "modules": [{
                "module": "Text",
                "content": "<h1 style='color:#FFFFFF'>Innovate Tech</h1><p style='color:#FFFFFF'>Building the future of AI and web</p>",
                "settings": { "text_orientation": "center" }
              }]
            }]
          }]
        },
        {
          "type": "regular",
          "rows": [ 
            { "columns": [ ... three columns each with a Blurb module ... ] } 
          ]
        },
        ...
      ]
    }
    ```
    
    (The actual output JSON would be more extensive.) After importing this JSON, the Divi Builder would have all these sections ready to publish on the homepage.
    
- **Example 2: Full Site Template**  
    **User Prompt:** _“Create a complete Divi theme builder template for a blog site. Include: a global header with a logo on the left and navigation menu on the right (simple, sticky); a global footer with 4 columns (about, links, newsletter signup form, social icons); a template for blog posts that has the post title in a fullwidth header section, the post content, and an author box at bottom; and a 404 page template with a custom message. Use Divi’s Theme Builder JSON format, and ensure each template is assigned correctly (header & footer global, post template applies to all posts, 404 for not found). Provide the output as a single JSON that can be imported into Divi Theme Builder. Only JSON, no commentary.”_  
    **Expected LLM Output:** The LLM would output a JSON containing multiple layout objects under a theme builder structure. It might have a structure like:
    
    ```json
    {
      "themeBuilder": {
        "globalHeader": { /* JSON for header layout */ },
        "globalFooter": { /* JSON for footer layout */ },
        "templates": [
          {
            "name": "All Posts",
            "assignedPosts": "all",
            "layout": { /* JSON for blog post template (sections/rows/modules) */ }
          },
          {
            "name": "404 Page",
            "assigned404": true,
            "layout": { /* JSON for 404 page content */ }
          }
        ]
      }
    }
    ```
    
    Within those, the header JSON would include a Section with a row for logo and menu (perhaps using an Image module for logo and a Menu module or Code module for the menu), styled to be sticky. The footer JSON would have four columns each with Text (and maybe Email Optin module for newsletter). The blog post template’s layout would use Divi’s Post Title Module (to dynamically show the post’s title and meta) and a Post Content Module (to pull in the post body), plus perhaps a sidebar if desired. The 404 template would have some text like “Page Not Found” and a button to go home. All of these would be contained in one file. After import, Divi’s Theme Builder would register a Global Header, Global Footer, a template for All Posts, and a 404 template, with conditions already set (all posts, 404). The user could then tweak text or design, but the heavy lifting is done.
    
- **Example 3: Component/Section Prompt** (for a more granular use-case)  
    **User Prompt:** _“Generate a Divi JSON snippet for a **call-to-action section** that I can import into an existing page. It should be a fullwidth section with a centered text and button: Big heading saying 'Get a Free Quote', sub-text, and a call-to-action button that opens a contact form. Use the site’s accent color #FF6600 for the button. Provide just the JSON for that section (one section with one column).”_  
    **Expected LLM Output:** In this case, the LLM would return JSON for just a single section layout. The snippet might look like:
    
    ```json
    {
      "sections": [{
        "type": "regular",
        "settings": {
          "padding": "50px 0px 50px 0px",
          "text_align": "center",
          "custom_css_section": ""
        },
        "rows": [{
          "columns": [{
            "modules": [
              {
                "module": "Text",
                "content": "<h2>Get a Free Quote</h2><p>Contact us today and receive a free consultation.</p>",
                "settings": {
                  "text_orientation": "center",
                  "module_alignment": "center"
                }
              },
              {
                "module": "Button",
                "content": "Get a Quote",
                "settings": {
                  "button_url": "#contact",
                  "button_alignment": "center",
                  "custom_button": true,
                  "button_text_color": "#ffffff",
                  "button_bg_color": "#FF6600",
                  "button_border_radius": "3px"
                }
              }
            ]
          }]
        }]
      }]
    }
    ```
    
    This JSON defines one section with a Text module and a Button module beneath it. The button uses a placeholder link “#contact” (which the user can later link to a form module or an anchor on the page) and the specified color. The `custom_button: true` and color fields ensure the Divi Button module gets the provided styling. The user can import this JSON via Divi Library and then insert the section into any page as needed.
    

Each of these examples demonstrates how the prompt guides the LLM to produce specific Divi-compatible JSON outputs. By following the structure and guidelines in this blueprint, the LLM can generate anything from a small section to an entire website’s layout in JSON format, ready to import, accelerating the web design process dramatically while staying within WordPress/Divi’s robust framework.