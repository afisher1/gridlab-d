# House E

  In this section, we describe the residential and small commercial building
  thermal load calculation, the modeling approach, major assumption and model
  testing and validation.

The major heat gains and losses that contribute to the building cooling or heating load consist of the following:

1. Conduction through exterior walls, roof, and glass
2. Heat gain/loss from infiltration of outside air through opening
3. Solar radiation through glass
4. Internal gains from lighting, people, equipment

Items 1, 2, and 3 are considered driven by the external source, while item 4 is consider internally generated. The thermal load can be either sensible or latent. Sensible load results in increase/decrease in the air temperature; latent load results in increase/decrease of water vapor, which increases/decreases the humidity. Items 1 and 3 are solely sensible, items 2 can be both sensible and latent, internal gains from lights are sensible and internal gains from people and equipment can be both sensible and latent.

In general, the amount of heat that must be removed (cooling load) or added (heating load) is not always equal to the amount of heat received or lost at a given time. The difference is a result of the heat storage and time lag effects \[citation]. Only a portion of the heat entering or leaving building actually heats/cools the room air immediately; the rest heats the building mass – the roof, walls, floors and building mass and air mass. The heat that is stored in the mass will result in heating/cooling load at a later time. So, the modeling approach will have to account for the storage effect.

To account for the storage effects when estimating the conduction gains from the exterior walls, roofs, and glass cooling load temperature difference method (CLTD) can be used:

<Latex>
  {`$x^n + y^n = z^n + P_{ref}$`}
</Latex>

<Latex>
  {`$x^n + y^n = z^n$`}
</Latex>