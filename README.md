import json
from logpy import Relation, facts, run, conde, var, eq
# To Check that if 'a' is the parent of 'b'
def parent(a,b):
    return conde([father(a,b)],[mother(a,b)])

# To Check that if 'a' is the grandparent of 'b'
def grandparent(a,b):
    temp=var()
    return conde((parent(a,temp),parent(temp,b)))

#To Check for sibling relationship between 'm' and 'n'  
def sibling(a,b):
    temp=var()
    return conde((parent(temp,a), parent(temp,b)))

# Check if m is n's uncle
def uncle(a,b):
    temp=var()
    return conde((father(temp,a),grandparent(temp,b)))

if __name__=='__main__':
    father=Relation()
    mother=Relation()   
f=open("C:\\Users\\ayush sharma\\Desktop\\relationships.json")
d=json.loads(f.read())
    #d=["father","mother"]
for item in d['father']:
    facts(father,(list(item.keys())[0],list(item.values())[0]))

for item in d['mother']:
    facts(mother,(list(item.keys())[0],list(item.values())[0]))

a=var()

    # Suresh's children
name="Suresh"
output=run(0,a,father(name,a))
print("\nList of " + name + "'s children:")
for item in output:
    print(item)

    # Mahesh's mother
name="Mahesh"
output=run(0,a,mother(a,name))
print("\n" + name + "'s mother:\n"+''.join(output))

    # Naresh's parents 
name="Naresh"
output=run(0,a,parent(a,name))
print("\nList of " + name + "'s parents:")
for item in output:
    print(item)

    # Manish's grandparents 
name="Manish"
output=run(0,a,grandparent(a,name))
print("\nList of " + name + "'s grandparents:")
for item in output:
    print(item)

    # Mohini's grandchildren 
name="Mohini"
output=run(0,a,grandparent(name,a))
print("\nList of " + name + "'s grandchildren:")
for item in output:
    print(item)

    # Deepak's siblings 
name="Deepak"
output=run(0,a,sibling(a,name))
siblings=[a for a in output if a != name]
print("\nList of " + name + "'s siblings:")
for item in siblings:
    print(item)

    # Tanya's uncles
name="Tanya"
name_father=run(0,a,father(a,name))[0]
output=run(0,a,uncle(a,name))
output=[a for a in output if a!= name_father]
print("\nList of " + name + "'s uncles:")
for item in output:
    print(item)

    # All spouses
m,n,o=var(),var(),var()
output=run(0,(m,n),(father,m,o),(mother,n,o))
print("\nList of all spouses:")
for item in output:
    print('Husband:', item[0], '<==> Wife:', item[1])

     
