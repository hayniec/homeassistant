# Dashboard Updates

I have split your `main.yaml` into multiple files to make it much easier to manage.

1.  **Split Files**: `main.yaml` has been converted into smaller, more manageable files, each representing a single view. These new files are located in the `dashboards/views/` folder.
2.  **Includes**: `main.yaml` now uses the `!include` directive to import the contents of each view sequentially.
3.  **Swipe Card Added**: I added a `custom:swipe-card` layout to `02_security.yaml` instead of the original grid. This will display your cameras in a horizontal slider to save space and make the UI look more premium.

### Next Steps:
* Ensure that you have the **Swipe Card** and **Mushroom** front-end integrations added via HACS.
* Restart your Home Assistant or reload your dashboards to see the changes.
* To edit any view, you can now open its corresponding file inside `dashboards/views/` rather than searching through the massive `main.yaml` file.
