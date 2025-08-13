# Divi Layout Skeletons

## 0) Minimal Page Wrapper
{
  "layout": {
    "metadata": {
      "et_builder_version": "{{divi_version}}",
      "exported_at": "{{ISO8601}}",
      "name": "{{layout_name}}"
    },
    "sections": []
  },
  "theme_builder": null,
  "global_presets": {},
  "asset_manifest": [],
  "notes": "Import via Divi > Divi Library (layouts) or Theme Builder (templates)."
}

---

## 1) Section: HERO (image bg + headline + button)
{
  "type": "regular",
  "settings": {
    "background_image": "{{hero_image_url_or_placeholder}}",
    "background_color": "{{hero_overlay_color}}",
    "padding": "100px 0 100px 0"
  },
  "rows": [
    {
      "settings": { "width": "80%", "max_width": "1280px", "align": "center" },
      "columns": [
        {
          "settings": { "text_align": "center" },
          "modules": [
            {
              "module_type": "et_pb_text",
              "content": "<h1>{{hero_title}}</h1><p>{{hero_subtitle}}</p>",
              "settings": { "text_color": "{{hero_text_color}}" }
            },
            {
              "module_type": "et_pb_button",
              "content": "{{cta_label}}",
              "settings": {
                "button_url": "{{cta_url}}",
                "custom_button": true,
                "button_bg_color": "{{accent_color}}",
                "button_text_color": "#ffffff",
                "button_alignment": "center"
              }
            }
          ]
        }
      ]
    }
  ]
}

---

## 2) Section: 3-Column Services (Blurbs)
{
  "type": "regular",
  "settings": { "padding": "60px 0" },
  "rows": [
    {
      "settings": { "column_layout": "1/3+1/3+1/3", "gutter": "2" },
      "columns": [
        { "settings": {}, "modules": [ { "module_type": "et_pb_blurb", "content": { "title": "{{service1_title}}", "body": "{{service1_desc}}", "icon": "{{icon_name_or_empty}}" }, "settings": {} } ] },
        { "settings": {}, "modules": [ { "module_type": "et_pb_blurb", "content": { "title": "{{service2_title}}", "body": "{{service2_desc}}", "icon": "{{icon_name_or_empty}}" }, "settings": {} } ] },
        { "settings": {}, "modules": [ { "module_type": "et_pb_blurb", "content": { "title": "{{service3_title}}", "body": "{{service3_desc}}", "icon": "{{icon_name_or_empty}}" }, "settings": {} } ] }
      ]
    }
  ]
}

---

## 3) Section: Call-to-Action (centered)
{
  "type": "regular",
  "settings": { "background_color": "{{cta_bg}}", "padding": "48px 0" },
  "rows": [
    {
      "settings": { "width": "860px", "align": "center" },
      "columns": [
        {
          "settings": { "text_align": "center" },
          "modules": [
            { "module_type": "et_pb_text", "content": "<h2>{{cta_title}}</h2><p>{{cta_sub}}</p>", "settings": {} },
            { "module_type": "et_pb_button", "content": "{{cta_label}}", "settings": { "button_url": "{{cta_url}}", "custom_button": true, "button_bg_color": "{{accent_color}}" } }
          ]
        }
      ]
    }
  ]
}

---

## 4) Section: Blog Listing (dynamic)
{
  "type": "regular",
  "settings": { "padding": "48px 0" },
  "rows": [
    {
      "settings": {},
      "columns": [
        {
          "settings": {},
          "modules": [
            {
              "module_type": "et_pb_blog",
              "content": null,
              "settings": {
                "posts_number": 6,
                "include_categories": "{{category_slug_or_all}}",
                "show_featured_image": true,
                "show_excerpt": true,
                "use_masonry": false
              }
            }
          ]
        }
      ]
    }
  ]
}

---

## 5) Section: Contact (native form)
{
  "type": "regular",
  "settings": { "padding": "60px 0" },
  "rows": [
    {
      "settings": { "column_layout": "1/2+1/2" },
      "columns": [
        {
          "settings": {},
          "modules": [
            { "module_type": "et_pb_text", "content": "<h2>Contact Us</h2><p>{{contact_copy}}</p>", "settings": {} }
          ]
        },
        {
          "settings": {},
          "modules": [
            {
              "module_type": "et_pb_contact_form",
              "content": {
                "fields": [
                  { "type": "input", "label": "Name", "required": true },
                  { "type": "email", "label": "Email", "required": true },
                  { "type": "textarea", "label": "Message", "required": true }
                ]
              },
              "settings": { "success_message": "Thanks! We’ll get back to you shortly." }
            }
          ]
        }
      ]
    }
  ]
}

---

## 6) Section: Gallery
{
  "type": "regular",
  "settings": { "padding": "48px 0" },
  "rows": [
    {
      "settings": {},
      "columns": [
        {
          "settings": {},
          "modules": [
            {
              "module_type": "et_pb_gallery",
              "content": {
                "images": [
                  { "src": "{{img1_url}}", "alt": "{{img1_alt}}" },
                  { "src": "{{img2_url}}", "alt": "{{img2_alt}}" },
                  { "src": "{{img3_url}}", "alt": "{{img3_alt}}" }
                ]
              },
              "settings": { "show_title": false, "show_caption": false, "columns": 3 }
            }
          ]
        }
      ]
    }
  ]
}

---

## 7) Section: Slider (testimonials/hero carousel)
{
  "type": "regular",
  "settings": {},
  "rows": [
    {
      "settings": {},
      "columns": [
        {
          "settings": {},
          "modules": [
            {
              "module_type": "et_pb_slider",
              "content": {
                "slides": [
                  { "heading": "{{slide1_title}}", "content": "<p>{{slide1_body}}</p>", "image": "{{slide1_img}}" },
                  { "heading": "{{slide2_title}}", "content": "<p>{{slide2_body}}</p>", "image": "{{slide2_img}}" }
                ]
              },
              "settings": { "show_arrows": true, "show_pagination": true, "automatic": false }
            }
          ]
        }
      ]
    }
  ]
}

---

## 8) Theme Builder: Global Header
{
  "sections": [
    {
      "type": "regular",
      "settings": { "padding": "12px 0", "background_color": "{{header_bg}}" },
      "rows": [
        {
          "settings": { "column_layout": "1/4+3/4", "vertical_align": "middle" },
          "columns": [
            { "settings": {}, "modules": [ { "module_type": "et_pb_image", "content": { "src": "{{logo_url}}", "alt": "{{brand_name}} logo" }, "settings": { "max_width": "160px" } } ] },
            { "settings": { "text_align": "right" }, "modules": [ { "module_type": "et_pb_menu", "content": null, "settings": { "menu": "{{primary_menu_name}}", "sticky": true } } ] }
          ]
        }
      ]
    }
  ]
}

---

## 9) Theme Builder: Global Footer (4 cols)
{
  "sections": [
    {
      "type": "regular",
      "settings": { "background_color": "{{footer_bg}}", "padding": "60px 0" },
      "rows": [
        {
          "settings": { "column_layout": "1/4+1/4+1/4+1/4" },
          "columns": [
            { "settings": {}, "modules": [ { "module_type": "et_pb_text", "content": "<h4>About</h4><p>{{about_blurb}}</p>", "settings": { "text_color": "{{footer_text}}" } } ] },
            { "settings": {}, "modules": [ { "module_type": "et_pb_text", "content": "<h4>Links</h4><ul><li><a href='{{url1}}'>{{label1}}</a></li></ul>", "settings": {} } ] },
            { "settings": {}, "modules": [ { "module_type": "et_pb_email_optin", "content": { "title": "Newsletter" }, "settings": {} } ] },
            { "settings": {}, "modules": [ { "module_type": "et_pb_social_media_follow", "content": { "networks": ["facebook","instagram","linkedin"] }, "settings": {} } ] }
          ]
        }
      ]
    }
  ]
}

---

## 10) Theme Builder Template: Blog Post Body
{
  "name": "All Posts",
  "applies_to": "post:*",
  "assign_on_import": true,
  "layout": {
    "sections": [
      { "type": "regular", "settings": { "padding": "60px 0" }, "rows": [ { "settings": {}, "columns": [ { "settings": {}, "modules": [ { "module_type": "et_pb_post_title", "content": null, "settings": { "show_meta": true, "show_thumbnail": true } }, { "module_type": "et_pb_post_content", "content": null, "settings": {} } ] } ] } ] }
    ]
  }
}

---

## 11) Theme Builder Template: 404
{
  "name": "404 Page",
  "applies_to": "404",
  "assign_on_import": true,
  "layout": {
    "sections": [
      { "type": "regular", "settings": { "padding": "100px 0", "text_align": "center" }, "rows": [ { "settings": {}, "columns": [ { "settings": {}, "modules": [ { "module_type": "et_pb_text", "content": "<h1>Page Not Found</h1><p>Try the menu or return <a href='/'>home</a>.</p>", "settings": {} } ] } ] } ] }
    ]
  }
}

---

## 12) Theme Builder Pack Shell (header/footer + templates)
{
  "global_header": {{see Global Header JSON}},
  "global_footer": {{see Global Footer JSON}},
  "templates": [ {{Blog Post Body JSON}}, {{404 JSON}} ]
}

---

## 13) Asset Manifest Pattern
[
  {
    "slot_id": "hero_bg",
    "purpose": "hero background",
    "alt": "Skyline at dusk over downtown",
    "prompt": "photo of city skyline at dusk, warm light, wide angle, minimal color noise",
    "target_size": "1920x1080",
    "format": "webp"
  }
]
