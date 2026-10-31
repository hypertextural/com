Links: [[Next.js]]

Zachary Rollyson:
	another annoying thing next did:

removed support for <Head> and they’re going to make you use their custom metadata API.
	So like, you used to be able to do this:

function IndexPage() {
  return (
    <div>
      <Head>
        <title>My page title</title>
      </Head>
      <p>Hello world!</p>
    </div>
  )
}
	now they want you to do this: 

export const metadata = {
  generator: 'Next.js',
  applicationName: 'Next.js',
  referrer: 'origin-when-cross-origin',
  keywords: ['Next.js', 'React', 'JavaScript'],
  authors: [{ name: 'Seb' }, { name: 'Josh', url: 'https://nextjs.org' }],
  creator: 'Jiachi Liu',
  publisher: 'Sebastian Markbåge',
  formatDetection: {
    email: false,
    address: false,
    telephone: false,
  },
}
	just another example of them making it less interoperable

Amanda Vacharat:
	That’s a stupid ugly change

Zachary Rollyson:
	apparently you can still use Head, their docs just pretend it doesn’t exist

Amanda Vacharat:
	To do like, a fundamental basic part of website making 

Zachary Rollyson:
	With how much control they’ve taken over rendering everything I’m not sure if it’s something you’ll be able to even circumvent after they yank Head from us.

Amanda Vacharat:
	What is “it’s” 

Zachary Rollyson:
	Oh I mean they might cut off access to the head.
	Like, you might not be able to edit the contents of <head> directly in their next version, unless you just fork the framework.