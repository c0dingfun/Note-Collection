# ASP.Net Core 

ActionResult
----

- The return Type (result) of all Controller class is ActionResult
- There are many classes derived from ActionResult, which implements the IActionResult (by the way)
- HTML
    + ViewResult - View()
    + PartialViewResult - PartialView()

- File
    + FileResult - File()

- Content

    + JsonResult
    + ContentResult
    + EmptyResult

- Errors

    + StatusCodeResult - StatusCode()
    + ForbidResult - Forbid()
    + ObjectResult - Object()
    + NotFoundResult - NotFound()

- Rediction

    + RedirectResult - Redirect()
    + RedirectToActionResult - RedirectToAction()
    + RedirectToPageResult - RedirectToPage()
    + RedirectToRouteResult - RedirectToRoute()
    + LocalRedirectResult - LocalRedirect()

- Security

    + ChallengeResult - Challenge()
    + SignInResult - SignIn()
    + SignOutResult - SignOut()

- Note: Don't need to normally instantiate these ActionResults; there are methods in ControllerBase or Extension methods that return the above ActionResult(s).

