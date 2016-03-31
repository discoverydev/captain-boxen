# Setup Notes

This is a new Boxen install. The goal is to bridge the gap between our current Boxen setup and OSX 10.11. 

Starting from fresh, so there's going to be a lot of stuff that needs to be updated and carried over from the Old N' Busted Boxen.

## Changes from Default
* Need to turn off the FDE requirement. (Either use --no-fde when invoking script or remove that bit from the code

## Ideas for how to do this
So we really need more than just a quick hack to get Boxen working on 10.11 - we need an overhaul of our whole setup. There's a lot of crufty stuff in the old repo, and it would be good to get some of that stuff separated out. 

Boxen should still be the sole manager of packages installed on a given Mac. It should also handle placing dotfiles (or other files, like the hosts file). Whatever can be easily installed with brew or brew cask belongs in boxen. Whatever configuration can be accomplished by putting files in places is also Boxen's wheelhouse. This will cover the majority of the configuration we need to do.

The VM scripts should be moved into their own repository.

DJ left a `migrate_repository` script that I think gets used to point to the new boxen when I switch it over, good to know.

`run_after_boxen` can still be used to get our tailored Android sdk, although I'm sure there will be more config necessary when we actually get into android stuff.

`add_plist_entry` - tempted to move plist into repo, let boxen copy it into place the same way we do dotfiles, and add the launchctl stuff to the end of the update boxen script or maybe into runafter.
   
instead of that big ugly url can we tap bigboybrew to get sshpass via cask??

## What Scripts Do:
add plist entry - pulls workstation files, copies the plist that runs boxen nightly into place. 
brew update pre bxoen - just updates brew. is this necessary? seems dumb that boxen doesn't do this already.
create boxen tar - tars up /opt/boxen. I don't think we'll need this anymore
migrate repository - think this is for switching the repo over when you do the kinda hot swap thing dj did when he updated 
nuke machine - still useful, gets lots of stuff that ./script/nuke wont. 
run after boxen - copies android sdk over and into correct spot, this seems to work fine, so we'll keep 
run before boxen - this is exclusively for copying over the boxen tar, probably getting ditched
set stash password - fine, but the order described in the confluence page is pretty problematic, gotta come up with a better way
start jenkins slave - still useful, still necessary. could we maybe have a different project for slaves? my guess is that we decided against this because it's more complicated than just putting everything on every machine.
update boxen - wraps ./script/boxen. Will need updates. 

### Stuff to port over
* Plist setup thing. That plist is in ./LaunchAgents - it probably can live in ./manifests/files. Are there other relevant files in the repo?
* ./certs - and the steps that install them
* run after boxen script still pretty much the same. I think run before can be dropped.
* modules/projects/manifests - we don't really use anything but workstation, but will probably keep all of these
* nginx stuff under modules/projects/templates/shared - not sure that we use this anymore, need to find out
* any discoverydev stuff under modules/people/manifests
* scripts - `add_plist_entry.sh, probably brew_update_pre_boxen - honestly probably all of them. update boxen will need changes too`


### Stuff That's Getting Cut
Dynatrace
Mockability
Kevin & DJ specific profiles


## Stuff I've Done

Got mac into fresh state 
Bootstrap a new boxen repo according to the instructions on the our-boxen main page
run ./script/boxen

Starting the porting over process. The first point of attack is the site.pp file. I've done pretty much a direct copy of that file. I'd like totry to break some of that out into modules. Also cut out the code that requires FDE. 

Ported over site.pp, lets see how it runs
Had to make some changes to npm packages, puppet broke the old api so there's a new syntax for declaring these deps. 

## TODO
Make this Mac an X-Man; hostname, background, gmail acct, ssh keys

## Current state
Was asked to put this on the back burner. Current state of the repo isn't awesome, still getting failures with something that's in site.pp . See attempt-log-3-31-16.log for the logs from the last run attempted before I dropped this. 

