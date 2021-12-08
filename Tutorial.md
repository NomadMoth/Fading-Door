# Fading Door Tutorial

## 1. Variables

Create two public variables, one for the door Mesh Renderer and the other for the door box collider:

    public MeshRenderer rend;
    public BoxCollider doorCollider;
    
## 2. Get Component
   
In the Start function, use GetComponent<MeshRenderer> command to allow it to be mreferenced.

    rend = GetComponent<MeshRenderer>();
    
## 3. Coroutines

Create two coroutines, one to fade the door out and the other to fade the door in.

IEnumerator FadeOut()
    {
        // This references the value of "f" and states that if it is more than or equal to 0.05, to then subtract 0.05f over the time specified in the function
        for (float f = 1f; f >= -0.05f; f -= 0.05f)
        {
            // This designates "c" as the reference for material colour
            Color c = rend.material.color;
            // This links the alpha channel of the material to "f"
            c.a = f;
            rend.material.color = c;
            // This disables the box collider, allowing movement through the object
            doorCollider.enabled = false;
            // The door will fade and the box collider disabled over the course of the specified time
            yield return new WaitForSeconds(0.05f);
        }
    }
    
    IEnumerator FadeIn()
    {
        for (float f = 0.05f; f <= 1; f += 0.05f)
        {
            Color c = rend.material.color;
            c.a = f;
            rend.material.color = c;
            doorCollider.enabled = true;
            yield return new WaitForSeconds(0.05f);
        }
    }
    
## 4. Triggers

Create an OnTriggerEnter function, within which there needs to be an if statement stating that when the player enters the trigger area, the FadeOut function will be called.

void OnTriggerEnter(Collider other)
    {
        if (other.tag == "Player")
        {
            StartCoroutine("FadeOut");
        }
    }
    
Create an OnTriggerExit function with an if statement stating that when the player exits the trigger area to call the FadeIn function.

void OnTriggerExit(Collider other)
    {
        if (other.tag == "Player")
        {
            StartCoroutine("FadeIn");
        }
    }
    
Note: Remember to ensure that the player has the correct tag ("Player") for the code to work.

## 5. Unity

Within the Unity editor, create an object to place the script on, such as a cube. Then, add another box collider, make it a trigger and enlarge it around the cube, and then
drag the cube mesh renderer and box colliders into the appropriate boxes in the inspector.

![Screenshot 2021-12-08 175727](https://user-images.githubusercontent.com/72862464/145259476-6e7180dd-dee0-41f9-ae3b-4c3778b8e153.jpg)
                                    
Now simply run the game, move the player in and out of the trigger to see it work.
