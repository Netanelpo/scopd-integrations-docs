# ChatGPT Assistant Integration Guide

This guide explains how to enable and use the built-in ChatGPT assistant in the SCOPD SIEM dashboard.

---

## 1. Open the ChatGPT assistant

1. In the dashboard, look at the **bottom-right corner** of the screen.
2. Click the **blue ChatGPT icon** to open the assistant panel.

---

## 2. Open the settings menu

1. In the opened assistant panel, click the **gear (⚙) icon** at the top of the dropdown/menu.
2. This opens the **ChatGPT / OpenAI settings** section.

---

## 3. Enter your OpenAI API token

1. In the settings panel, find the field labeled something like **OpenAI token** / **OpenAI API key**.
2. Paste your **OpenAI API token** into this field.

   * It usually looks like: `sk-...`
3. Confirm/save the settings (for example, by clicking **Save** or closing the settings if they are auto-saved).

After this, the assistant can use your own OpenAI account to answer queries.

---

## 4. Using the ChatGPT assistant

Once the token is saved, you can start using the assistant:

* Type any question in the **chat input** at the bottom of the assistant panel.
* You can ask about:

  * SCOPD SIEM usage
  * Wazuh configuration and rules
  * general SIEM/SOC topics
  * step-by-step instructions, troubleshooting, etc.

### Image support

⚠ **Important:** You can upload screenshots in two ways:

1. Click the **upload/attachment button** in the chat and select an image file.
2. Or simply **paste from the clipboard** (e.g. `Ctrl + V` after taking a screenshot).

The assistant can **analyze and recognize content on images** (screenshots, error messages, dashboards, etc.).

---

## 5. Choose the ChatGPT version

At the top of the assistant panel, to the right of the label **“AI assistant”**, there is a **dropdown with model versions**.

1. Click the dropdown to open the list of available ChatGPT versions.
2. Select the version you want to use (for example, a “faster” or a “more accurate” model, depending on what is available in your setup).

The selected version will be used for all subsequent replies until you change it.

---

## 6. Handling API errors

If you get an error like **`api error`** when sending a request:

1. **Check your token spelling:**

   * Make sure there are no extra spaces before or after the key.
   * Confirm you copied the full key (it should usually start with `sk-`).
2. Verify that:

   * The token is **still active** in your OpenAI account.
   * The key has **enough quota / billing is valid** (if applicable).

If the token is invalid or expired, replace it with a new one in the settings (gear icon → token field) and try again.

