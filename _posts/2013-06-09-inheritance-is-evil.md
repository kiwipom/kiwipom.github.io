---
layout: post
title: The inherent evils of Inheritance
tags:
- inheritance
- dogmatism
---

These days, it feels like every time I open twitter someone is telling me I should *on every occasion* choose composition over inheritance.


I learnt how to inherit objects in C++ back in 1999, and I've been using it in my code pretty much ever since. So what's the beef? Have I been spreading evil all this time?

### Inheritance *can* be evil

Sure - if you go looking, you can find terrible implementations of good design patterns. I call your attention to exhibit A: WPF controls.

    System.Object
      System.Windows.Threading.DispatcherObject
        System.Windows.DependencyObject
          System.Windows.Media.Visual
            System.Windows.UIElement
              System.Windows.FrameworkElement
                System.Windows.Controls.Control
                  System.Windows.Controls.Primitives.TextBoxBase
                    System.Windows.Controls.TextBox
                      System.Windows.Controls.Primitives.DatePickerTextBox
                      System.Windows.Controls.Ribbon.RibbonTextBox
                      
/me raises eyebrow. Really?

But that doesn't mean that I'm *never* allowed to use inheritance, does it?

Consider an API that your product exposes to your B2B partners. You're using xml-based web services and you want to let your partners create a profile and update a profile.

---

**nerd-sniper**: *xml? What like in olden days? you should be using JSON / REST*

**me**: *Well, let's call "ripping out our mature, well-defined web services layer and completely fucking rewriting it with a similar (admittedly nicer) technology" plan 'B', shall we?*

---


So I want to create a couple of request types. They're fundamentally the same, but on the 'create' I don't need a `ProfileId` and on the 'update' I do.

We could do this with composition, sure. 

    [DataContract]
    public class CreateProfileRequest
    {
    	[DataMember]
    	public Profile Profile { get; set; }
    }


	[DataContract]
	public class UpdateProfileRequest
	{
		[DataMember]
		public int ProfileId { get; set; }
		
		[DataMember]
		public ProfileDetails Profile { get; set; }
	}
	
	
But we could *equally* do this with inheritance:

	[DataContract]
	[KnownType(typeof(CreateProfileRequest))]
	[KnownType(typeof(UpdateProfileRequest))]
    public abstract class ProfileRequest
    {
    	[DataMember]
    	public Profile Profile { get; set; }
    }
    
	[DataContract]
    public class CreateProfileRequest : ProfileRequest
    {}
    
	[DataContract]
    public class UpdateProfileRequest : ProfileRequest
    {
		[DataMember]
		public int ProfileId { get; set; }
    }
   
   
I happen to like this because now the core bits of getting requests into the system are in a single place, and not two.

But - ***and this is the essence of this post*** - It IMHO really doesn't matter which way you choose to write these classes. They both work, and they're both simple to maintain.


Inheritance isn't *inherently* evil. Dogmatism for the sake of dogmatism, just may be ;-)



Go on then, Corey. Tell me why I'm wrong…

