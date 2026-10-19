# Mini web app: test checklist

Go through every line before you hand over the file. Report it as a ticked list.

## Works
- [ ] Opens by double-clicking `index.html`, with no console errors
- [ ] The main task works end to end (add → see result → totals update)
- [ ] Data is still there after reloading the page
- [ ] Still works when storage is blocked (it just doesn't save)
- [ ] Export CSV downloads a file that opens in Excel or Numbers (when the app stores a list)

## Input
- [ ] Empty, zero, negative and non-number inputs show an inline message and add nothing
- [ ] Text with `<b>` or `<script>` shows as plain text
- [ ] Delete asks for a confirmation

## Looks
- [ ] 360px wide: no horizontal scroll, buttons at least 44px tall
- [ ] Desktop width: the content is centred, max width around 960px
- [ ] Light and dark mode both readable (follows the system setting)
- [ ] Money shows as `1 234,50 zł` (pl-PL), dates as `DD.MM.YYYY`
- [ ] Empty state shows a helpful line, not a blank area

## Access
- [ ] Every input has a label, and the focus ring is visible when using Tab
- [ ] Meaning is never shown by colour alone (for example, "over budget" also has text)
