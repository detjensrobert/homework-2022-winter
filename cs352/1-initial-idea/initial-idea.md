# CS 352 Module 1: Initial Idea

## Robert Detjens

---

## Summary

Landlords commonly need to track the occurrences of bedbugs across their holdings. This application will serve as a way
for landlords and tenants to track bedbug infestations and catch them in the early stages before costly fumigation is
needed.

## Introduction

Bedbug infestations are very costly and time-consuming to treat once a colony has been established, and treatment can
have side effects for furniture and such in the treatment area. If the presence of bedbugs can be caught before it gets
out of control, treatment is much simpler.

There is a opportunity for a service that in essence offers bedbug traps-as-a-service; where a landlord can arrange for
traps to be delivered to clients, clients to monitor the traps and report back pictures of the traps to their landlords.

While there are services that allow for user-provided bedbug reports, they do not offer trap ordering and per-trap
tracking along with landlord integration.

## Interfaces

In terms of user interfaces, this application needs to:

- provide a way for landlords to make an account with the service
- provide a way for landlords to order bedbug traps for their tenants
- provide a way for tenants to submit pictures of the traps
- provide a way for landlords to examine pictures of tenants' pictures

In further detail:

- Account management
  - Login & signup flow: Landlords will need an account for the service to associate traps with.
  - Account dashboard: This page will show all currently-deployed traps a landlord has ordered and had delivered to
    tenants.
  - Account settings page: The landlord needs some way to configure their locations and ordering information.
- Trap management
  - Trap ordering flow: Traps need to be ordered somehow, with options for the quantity and delivery location for each
    batch of traps.
  - Trap location configuration page: If not configured when ordering, a landlord will need to associate each trap with
    a property, and possibly a room in the property if the tenant provides that information.
  - Submitted image page: The landlord needs to have a way to view the pictures submitted by tenants in order to examine
    them for bedbugs.
- Tenant submission
  - Trap location configuration page: The tenant should be able to configure the location of where they installed each
    trap in their apartment.
  - Trap feedback page: The landlord needs a way to leave feedback on submitted traps, such as whether there is a
    problem with the picture or if it does indeed have bedbugs and what the next steps will be to resolve that.

## Challenges & Conclusion

While this may seem like a very niche product, I have some relatives who are landlords and could use a service like
this.

If we were actually implementing this as a proper application instead of just a series of mockups (as I assume we will
be doing in this class), several parts of this would pose some challenges but have possible solutions:

- Associating a certain trap with a certain landlord is tricky, but could be solved with a QR code or similar on the
  trap with a unique identifier. However, this poses another issue:
- Pictures submitted by the tenant need to be clear enough that the QR code needs to be read. This may be able to be
  done automatically with image processing libraries, but may not be foolproof for the rest of the trap.
- While there needs to be a landlord account due to associating properties and payment information, tenants would
  ideally not need to create an account and just submit images somewhat anonymously to the service. The QR code tracking
  would then be able to associate the image / trap with the correct landlord, but this does not provide a way for the
  landlord to provide feedback on the images.
- The image submission process should be as easy as possible for tenants, since not everyone is tech-literate. There are
  several possible mechanisms to for submitting images, such as a full mobile app in order to submit or simply texting
  pictures to a number.
