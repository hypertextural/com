# Performance, accessibility, best practices & SEO

This report mainly concerns performance issues but I would like to hit on these other areas first because they are smaller and easier to overlook if I bury them at the end.

## Accessibility

Your pages are generally decently accessible but could use some improvements. Most issues are minor, quick fixes that would make a big difference to users (like being able to zoom in on text). The most challenging issue is contrast: you use a light gray for some links and meta-information that does not have sufficient contrast with a white background—this *could* be an easy fix, depends on how attached everyone is to that gray and how much changing it would upset the design balance.

## Best practices

These are minor security issues such as enabling HSTS, enforcing stronger content security policies; and small JavaScript bugs.

## SEO

At least some pages are missing meta descriptions, and some links do not have particularly descriptive text (e.g. "read more")—the former is generally more important unless you have *many* of the latter.

## Performance

These findings include data I gathered using the Lighthouse tool, which simulates mobile connectivity and processing power, and Google's Core Web Vitals, which are metrics gathered "in the wild" by Google on real user devices.

### Lighthouse metrics

The broad overview of all this is: 1) get rid of your loader; 2) try to get scripts & styles better prioritized; 3) remove plugins as possible; 4) defer loading off-screen images; 5) optimize image formats.
#### Delete the loading spinner!

I want to be sure to highlight this here because it's the easiest, highest impact thing you can do and it's on every page: ***delete the loading spinner!*** It isn't doing anything helpful. All of your content is obscured by it for any new visitor who is downloading all your scripts and styles for the first time. People will get frustrated and leave, especially anyone on a phone or with a bad connection. This has an outsize impact on the economically disadvantaged. Google will down-rank you for this. 

#### Home page

##### Desktop

The performance scores for this were the best of pages tested. Of greatest significance was the score for Cumulative Layout Shift. When the webpage loads there is is just the main hero text in an ~80% wide orange box; then all text is hidden for a moment; then the orange takes over the whole page and the text comes in along with the navigation text and the logo. All this is interpreted by the browser as layout shift. You can't see it on your site now because of the spinning loader, but the browser can.

The following images illustrate the layout shift.

![[Screenshot 2025-03-16 at 1.25.24 PM.png|First the page is a centered orange block with the hero text, but no nav, logo, or subtitle.]]

![[Screenshot 2025-03-16 at 1.25.12 PM.png|Then the orange expands, logo & search icon are visible, all text is covered (this is particularly bad!) and the BG image starts to load.]]

The final layout shift comes as the hero "title", "subtitle", and navigational links render back in.

Google weights this metric heavily because a user who is waiting for a page to load while the layout is shifting is likely to have the frustrating experience of that page jumping around while they're trying to read or otherwise interact with it. It's also important to remember that ***the text on your pages is the most important thing***, the text should be visible immediately, should not be covered, and should not move.

Scores for Accessibility, Best Practices and SEO averaged at 82%.

##### Mobile

Performance scores on mobile were significantly worse, with the total coming in at just 35. Unsurprisingly, Cumulative Layout Shift (CLS) was still an issue. What really brought the score down for mobile was *First* Contentful Paint (FCP) and *Largest* Contentful Paint (LCP).

FCP is poor because there are too many blocking resources loading too slowly. Blocking resources are those which prevent the rendering of content. These are styles and scripts that the HTML has told the browser must be loaded before rendering. Ways to improve this include eliminating used assets, loading assets *after* content is rendered, and inlining critical assets.

LCP is poor for several reasons but generally they fall within two categories. A disproportionate amount of time passed between when the website begins loading and when the resources of the largest content area (the big orange hero element) begin to load (i.e., something _else_ is loading during this time). The other main category is that the resources that make up that content simply take a long time to load—especially the background image and the scripts & styles that load early. There is some savings to be had in better compressing images, but I think the biggest thing is scripts & styles are loaded.

#### Sex & Relationships
##### Desktop

This actually has a pretty good performance score on desktop. There aren't a lot of images or plugins active on this page so it loads pretty quick. Still there are some places it could be more efficient, but it's not where I would focus. 

Scores for Accessibility, Best Practices and SEO are still a little low.

##### Mobile

The page scores poorly on mobile. The amount of scripts and styles being loaded are very out of line with the actual content being loaded. Mobile phones have considerably less processing power so the impact is felt a lot worse here. 

#### Transitional Kindergarten 101

This page is the same story as the other two.

### Core Web Vitals

Google's Core Web Vitals are data gathered from real users devices. This is important because it reflects who is actually using your website whereas Lighthouse metrics are simulated. They do tell the same story, however.

![[Screenshot 2025-03-16 at 5.55.13 PM.png]]

For desktop we see a mix with about 75% users getting a sub-optimal experience and 25% getting a good experience. The core issue for that 75% is again Cumulative Layout Shift and Largest Contentful Paint.

![[Screenshot 2025-03-16 at 5.54.50 PM.png]]

Results on mobile are much worse, where **100% of user experiences in the sample data are rated poor**. The biggest driver here is again Largest Contentful Paint.

