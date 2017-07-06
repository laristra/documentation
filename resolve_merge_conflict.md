### How to Resolve a Merge Conflict
One of the reasons for continuous integration is to avoid merge conflict. However, if two people edit copies of a file and attempt to merge them into the main branch, they will have to fix the conflicts. For example, part of the mytan.py file was originally like this:

    if __name__ == "__main__":

        arg = sys.argv[1].
        
        print "Tan of %s is %s" % (arg, float(mytan(arg)))
        
    
Edits were made on two separate copies of the mytan.py file. The first was this:

    if __name__ == "__main__":  `

        print "Tan of %s is %s" % (arg, mytan(float(sys.argv[1])))`
    
The second was this:

    if __name__ == "__main__":`

        arg=float(sys.argv[1])`
    
        print "Tan of %s is %s" % (arg, mytan(arg))`

Integration caused a merge conflict that looked like this:

    if __name__ == "__main__":`

    <<<<<<< HEAD`

        print "Tan of %s is %s" % (arg, mytan(float(sys.argv[1])))`
  
    =======`

        arg=float(sys.argv[1])`
   
        print "Tan of %s is %s" % (arg, mytan(arg))`
    
    >>>>>>> 9d7863ddb992bc06aa9d243225ff875e7c15b3f8`

To resolve, choose which of the two options to delete and keep the other, using `git add`, `git commit`, and `git push`.
