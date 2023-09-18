---
title: Custom Dialogs
layout: default
order: 10
---
# FAQ about UI and .Net
If existing [Syrve POS dialogs](ViewManager.html "ViewManager") are not enough, the plugin can show user-defined dialogs, however, certain specifics should be taken into account.

First, the plugin default host process does not include the [STA thread](https://msdn.microsoft.com/library/ms809971.aspx "Understanding and Using COM Threading Models"). required for the UI. Though the plugin must create it. Sample code:

```cs
ctor()
{
    var windowThread = new Thread(EntryPoint);
    windowThread.SetApartmentState(ApartmentState.STA);
    windowThread.Start();
}
...
private void EntryPoint()
{
    Window window = new MyWindow();
    window.ShowDialog();
}
```

Second, by default, a background process dialog has no focus, therefore, input events pertain to the previously active dialog, i.e. Syrve POS dialog. According to [Microsoft](https://devblogs.microsoft.com/oldnewthing/20090220-00/?p=19083 "Foreground activation permission is like love: You can’t steal it, it has to be given to you"), the app cannot be foreground on its own, it’s only the previously active dialog that can do it, or it is the user who can foreground the app. However, the latter can be pre-programmed:

```cs
public static void ClickWindow(Window wnd)
{
    try
    {
        var wih = new WindowInteropHelper(wnd);
        WinApi.RECT rect;
        WinApi.GetWindowRect(new HandleRef(wnd, wih.Handle), out rect);
        var x = rect.Left + (rect.Right - rect.Left) / 2;
        var y = rect.Top + (rect.Bottom - rect.Top) / 4;
        WinApi.LeftMouseClick(x, y);
    }
    catch (Exception) { }
}
```

Third, the plugin dialog, being an independent window, can be put behind the Syrve POS window. The Always On Top mode [cannot solve](https://social.msdn.microsoft.com/Forums/en-US/fb4a7d5f-c98b-461f-a527-7d5dd4cd03e6/multiple-topmost-windows?forum=wpf "Multiple Topmost Windows") the issue, as other topmost windows can be present, including the Syrve POS app itself. The plugin can link its dialog to the Syrve POS window as a child dialog via the `SetParent` WinAPI function. Although the Syrve POS window hwnd is not published in the API, the plugin can [find](https://stackoverflow.com/questions/10676649/attach-window-to-window-of-another-process) it on its own.




